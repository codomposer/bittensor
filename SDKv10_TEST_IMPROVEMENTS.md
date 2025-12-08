# SDKv10 Test Improvement Recommendations

This document outlines specific, necessary test improvements for the SDKv10 branch based on a comprehensive review of the existing test suite and source code.

---

## 1. Unit Tests - Missing Critical Test Cases

### 1.1 `add_stake_extrinsic` - Safe Staking Mode Tests

**File:** `tests/unit_tests/extrinsics/asyncex/test_staking.py`

**Current State:** Only `test_set_auto_stake_extrinsic` exists.

**Missing Tests:**
```python
# test_add_stake_extrinsic_safe_staking_with_price_tolerance
- Test safe_staking=True with rate_tolerance parameter
- Verify limit_price calculation with tolerance
- Test allow_partial_stake=True vs False behavior

# test_add_stake_extrinsic_insufficient_balance
- Test when amount > old_balance - existential_deposit
- Verify amount is adjusted correctly to prevent account death

# test_add_stake_extrinsic_mev_protection_enabled
- Test MEV protection path (submit_encrypted_extrinsic call)
- Verify correct parameters passed to MEV Shield

# test_add_stake_extrinsic_response_data_structure
- Verify response.data contains balance_before, balance_after, stake_before, stake_after
- Verify transaction_tao_fee and transaction_alpha_fee are set correctly
```

### 1.2 `add_stake_multiple_extrinsic` - Missing Entirely

**File:** `tests/unit_tests/extrinsics/asyncex/test_staking.py`

**Missing Tests:**
```python
# test_add_stake_multiple_extrinsic_success
- Test staking to multiple hotkeys in single transaction
- Verify batch call composition

# test_add_stake_multiple_extrinsic_partial_failure
- Test behavior when some stakes succeed and others fail
- Verify error aggregation and reporting

# test_add_stake_multiple_extrinsic_empty_list
- Test with empty hotkey/amount lists
- Verify appropriate error handling
```

### 1.3 `unstake_extrinsic` - Missing Tests

**File:** `tests/unit_tests/extrinsics/asyncex/test_unstaking.py`

**Missing Tests:**
```python
# test_unstake_extrinsic_safe_unstaking
- Test safe_unstaking=True parameter
- Verify limit_price calculation for unstaking

# test_unstake_extrinsic_unstake_all
- Test unstaking full balance
- Verify Balance(0) result after full unstake

# test_unstake_extrinsic_locked_stake_handling
- Test behavior when stake has locked portion
- Verify appropriate error/warning
```

### 1.4 MEV Shield Extrinsic Unit Tests

**File:** Create `tests/unit_tests/extrinsics/asyncex/test_mev_shield.py`

**Missing Tests:**
```python
# test_submit_encrypted_extrinsic_success
- Test successful encryption and submission
- Verify commitment and ciphertext generation

# test_submit_encrypted_extrinsic_sign_with_hotkey
- Test signing with hotkey instead of coldkey
- Verify correct signer selection

# test_wait_for_extrinsic_by_hash_success
- Test successful extrinsic discovery in subsequent block
- Verify correct block polling logic

# test_wait_for_extrinsic_by_hash_decryption_failure
- Test markDecryptionFailed event detection
- Verify failure handling

# test_wait_for_extrinsic_by_hash_timeout
- Test behavior when extrinsic not found within timeout_blocks
- Verify None return

# test_submit_encrypted_extrinsic_invalid_signer
- Test with sign_with parameter not in ("coldkey", "hotkey")
- Verify AttributeError raised
```

### 1.5 `move_stake_extrinsic` - No Unit Tests

**File:** Create or update `tests/unit_tests/extrinsics/asyncex/test_move_stake.py`

**Missing Tests:**
```python
# test_move_stake_extrinsic_same_subnet
- Test moving stake within same subnet to different hotkey

# test_move_stake_extrinsic_cross_subnet
- Test moving stake between different subnets (swap_stake)

# test_transfer_stake_extrinsic
- Test transfer stake to different coldkey
- Verify proper authorization checks
```

---

## 2. AsyncSubtensor Method Tests

### 2.1 `tests/unit_tests/test_async_subtensor.py` - Missing Tests

```python
# test_get_stake_with_locked_amount
- Test get_stake returns correct locked vs unlocked amounts
- Verify Balance unit handling per netuid

# test_sim_swap_accuracy
- Test sim_swap calculation accuracy
- Verify tao_fee and alpha_fee calculations

# test_get_subnet_hyperparameters_caching
- Test reuse_block parameter behavior
- Verify block hash caching works correctly

# test_compose_call_batch
- Test batch call composition
- Verify multiple calls are bundled correctly

# test_sign_and_send_extrinsic_retry_on_network_error
- Test retry behavior on transient network failures
- Verify exponential backoff

# test_get_delegates_lite_vs_full
- Test DelegateInfoLite vs full DelegateInfo retrieval
- Verify data consistency

# test_query_runtime_api_error_handling
- Test behavior when runtime API returns error
- Verify ChainError propagation
```

---

## 3. Chain Data Parsing Tests

### 3.1 `tests/unit_tests/chain_data/` - Missing Tests

**File:** Create `tests/unit_tests/chain_data/test_stake_info.py`
```python
# test_stake_info_from_dict_valid
- Test StakeInfo.from_dict with valid chain data
- Verify Balance unit is set correctly per netuid

# test_stake_info_from_dict_missing_fields
- Test handling of missing optional fields
- Verify default values

# test_stake_info_equality
- Test __eq__ comparison between StakeInfo objects
```

**File:** Create `tests/unit_tests/chain_data/test_dynamic_info.py`
```python
# test_dynamic_info_tao_to_alpha_with_slippage
- Test tao_to_alpha_with_slippage calculation
- Verify slippage calculation accuracy

# test_dynamic_info_alpha_to_tao_with_slippage
- Test reverse conversion accuracy
- Edge cases: zero values, maximum values

# test_dynamic_info_price_calculation
- Test price property calculation
- Verify consistency with manual calculation
```

**File:** Create `tests/unit_tests/chain_data/test_crowdloan_info.py`
```python
# test_crowdloan_info_from_dict
- Test CrowdloanInfo parsing from chain data
- Verify all fields populated correctly

# test_crowdloan_constants_from_dict
- Test CrowdloanConstants parsing
```

---

## 4. Error Handling Tests

### 4.1 `tests/unit_tests/test_errors.py` - Missing Tests

```python
# test_all_chain_error_subclasses_registered
- Verify all ChainError subclasses are in _ChainErrorMeta._exceptions
- Ensures from_error can instantiate all known errors

# test_chain_error_inheritance_chain
- Test ChainTransactionError -> ChainError -> SubstrateRequestException
- Verify isinstance checks work correctly

# test_stake_error_subclasses
- Test NotDelegateError, StakeError relationships
- Verify error message propagation

# test_registration_error_scenarios
- Test RegistrationNotPermittedOnRootSubnet
- Test NotRegisteredError

# test_rate_limit_errors
- Test TxRateLimitExceeded and DelegateTxRateLimitExceeded
- Verify error categorization
```

---

## 5. Stream Module Tests

### 5.1 `tests/unit_tests/test_stream.py` - Missing Edge Case Tests

```python
# test_streaming_response_with_large_chunks
- Test streaming with chunks > 64KB
- Verify memory efficiency

# test_streaming_response_connection_drop_midstream
- Test behavior when client disconnects during stream
- Verify proper cleanup

# test_concurrent_streaming_responses
- Test multiple concurrent streaming responses
- Verify thread safety and isolation

# test_streaming_synapse_timeout_handling
- Test timeout behavior during streaming
- Verify partial response handling
```

---

## 6. Dendrite/Axon Communication Tests

### 6.1 `tests/unit_tests/test_dendrite.py` - Missing Tests

```python
# test_dendrite_call_with_custom_timeout
- Test timeout parameter override
- Verify timeout propagation to aiohttp

# test_dendrite_call_network_latency_simulation
- Test behavior under high latency conditions
- Verify timeout handling works correctly

# test_dendrite_retry_on_5xx_errors
- Test retry behavior on server errors
- Verify exponential backoff implementation

# test_dendrite_certificate_verification
- Test SSL certificate handling
- Verify certificate chain validation
```

### 6.2 `tests/unit_tests/test_axon.py` - Missing Tests

```python
# test_axon_concurrent_request_handling
- Test handling multiple simultaneous requests
- Verify thread pool utilization

# test_axon_request_body_size_limit
- Test large payload handling
- Verify appropriate 413 responses

# test_axon_nonce_replay_attack_prevention
- Test nonce window validation
- Verify duplicate nonce rejection

# test_axon_middleware_exception_handling
- Test exception handling in middleware pipeline
- Verify error response format
```

---

## 7. Integration Tests

### 7.1 `tests/integration_tests/` - Missing Tests

**File:** Create `tests/integration_tests/test_stake_operations_integration.py`
```python
# test_stake_add_and_get_consistency
- Test add_stake followed by get_stake
- Verify balance consistency

# test_stake_multiple_hotkeys_same_coldkey
- Test staking to multiple hotkeys
- Verify total stake calculation
```

**File:** Create `tests/integration_tests/test_mev_shield_integration.py`
```python
# test_mev_shield_with_mock_substrate
- Test full MEV Shield flow with mock substrate
- Verify encryption/decryption pipeline
```

---

## 8. E2E Test Improvements

### 8.1 `tests/e2e_tests/test_staking.py` - Missing Scenarios

```python
# test_stake_slippage_protection
- Test safe_staking with actual price movements
- Verify slippage rejection works

# test_unstake_with_locked_funds
- Test unstaking when portion is locked
- Verify only unlocked portion is unstakable

# test_stake_rate_limit_enforcement
- Test rate limit between consecutive stakes
- Verify TxRateLimitExceeded error
```

### 8.2 `tests/e2e_tests/test_weights.py` - Missing Tests

**File:** Create `tests/e2e_tests/test_weights.py`
```python
# test_set_weights_as_validator
- Test weight setting by registered validator
- Verify weights applied on chain

# test_commit_reveal_weights_full_cycle
- Test commit, wait for reveal round, reveal
- Verify weights applied after reveal

# test_timelocked_weights_early_reveal_failure
- Test attempting reveal before reveal round
- Verify rejection

# test_weights_rate_limit
- Test WeightsSetRateLimit enforcement
- Verify appropriate error
```

### 8.3 `tests/e2e_tests/test_children.py` - Create New

```python
# test_set_children_single_child
- Test setting single child hotkey
- Verify child receives emission share

# test_set_children_proportion_distribution
- Test multiple children with different proportions
- Verify emission distribution

# test_set_children_exceeds_max_children
- Test setting more than 5 children
- Verify TooManyChildren error

# test_set_children_insufficient_stake
- Test setting children without enough stake
- Verify NotEnoughStakeToSetChildkeys error
```

---

## 9. Utility Function Tests

### 9.1 `tests/unit_tests/utils/test_balance.py` - Missing Tests

```python
# test_balance_arithmetic_with_different_netuids
- Test adding/subtracting balances from different netuids
- Verify unit preservation or conversion

# test_balance_from_rao_precision
- Test conversion from rao preserves precision
- Edge cases: very small values, maximum values

# test_balance_set_unit_idempotency
- Test calling set_unit multiple times
- Verify consistent behavior
```

### 9.2 `tests/unit_tests/utils/test_liquidity_utils.py` - Missing Tests

```python
# test_price_to_tick_edge_cases
- Test boundary conditions for price_to_tick
- Verify tick range constraints

# test_calculate_fees_accuracy
- Test fee calculation against known values
- Verify percentage accuracy

# test_liquidity_position_validation
- Test LiquidityPosition with invalid ranges
- Verify error handling
```

---

## 10. Configuration and Settings Tests

### 10.1 `tests/unit_tests/test_config.py` - Missing Tests

```python
# test_config_env_variable_override
- Test environment variable configuration
- Verify precedence order

# test_config_invalid_network_handling
- Test invalid network name handling
- Verify appropriate error/fallback

# test_config_endpoint_validation
- Test URL validation for chain endpoints
- Verify ws:// and wss:// handling
```

---

## Summary of Priority Improvements

### High Priority (Core Functionality)
1. MEV Shield unit tests (Section 1.4)
2. Safe staking mode tests (Section 1.1)
3. Move stake tests (Section 1.5)
4. Chain data parsing tests (Section 3)

### Medium Priority (Error Handling & Edge Cases)
1. Error handling completeness (Section 4)
2. Stream edge cases (Section 5)
3. Dendrite retry/timeout tests (Section 6)

### Lower Priority (Nice to Have)
1. Balance arithmetic tests (Section 9.1)
2. Configuration tests (Section 10)
3. Liquidity utility tests (Section 9.2)

---

## Implementation Notes

- All new tests should follow the existing pytest patterns
- Use `mocker` fixture for mocking (pytest-mock)
- Mark async tests with `@pytest.mark.asyncio`
- Use parametrized tests where multiple scenarios can share test logic
- Maintain test isolation - each test should be independent
- Add docstrings explaining what each test verifies

---

*Generated from SDKv10 branch analysis on 2024-12*
