# SDKv10 Test Improvements Roadmap

This document identifies areas for improvement in the SDKv10 test suite, focusing on **new test scripts to create** and **new test functions to add** rather than modifying existing tests.

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [New Test Scripts to Create](#new-test-scripts-to-create)
   - [Critical Priority - Chain Data Module](#critical-priority---chain-data-module)
   - [Critical Priority - Missing Extrinsics](#critical-priority---missing-extrinsics)
   - [High Priority - Pallet Operations](#high-priority---pallet-operations)
   - [High Priority - Extras Module](#high-priority---extras-module)
   - [Medium Priority - Core Utilities](#medium-priority---core-utilities)
3. [New Test Functions to Add to Existing Tests](#new-test-functions-to-add-to-existing-tests)
   - [test_axon.py Additions](#test_axonpy-additions)
   - [test_dendrite.py Additions](#test_dendritepy-additions)
   - [test_metagraph.py Additions](#test_metagraphpy-additions)
   - [test_subtensor.py Additions](#test_subtensorpy-additions)
   - [test_stream.py Additions](#test_streampy-additions)
   - [test_synapse.py Additions](#test_synapsepy-additions)
4. [Test Coverage Statistics](#test-coverage-statistics)
5. [Implementation Priority Matrix](#implementation-priority-matrix)

---

## Executive Summary

The SDKv10 test suite currently has **83 test files** with **931 test functions**. This analysis identifies:

- **92+ new test files** that should be created
- **185+ new test functions** to add to existing test files
- Focus areas: Chain data, async extrinsics, error handling, edge cases

---

## New Test Scripts to Create

### Critical Priority - Chain Data Module

**Location:** `tests/unit_tests/chain_data/`

Currently, only 2 of 25 chain data modules have tests. The following test files need to be created:

| New Test File | Source Module | Priority | Estimated Tests |
|--------------|---------------|----------|-----------------|
| `test_axon_info.py` | `bittensor/core/chain_data/axon_info.py` | Critical | 15-20 |
| `test_chain_identity.py` | `bittensor/core/chain_data/chain_identity.py` | Critical | 10-15 |
| `test_crowdloan_info.py` | `bittensor/core/chain_data/crowdloan_info.py` | Critical | 15-20 |
| `test_delegate_info.py` | `bittensor/core/chain_data/delegate_info.py` | Critical | 20-25 |
| `test_delegate_info_lite.py` | `bittensor/core/chain_data/delegate_info_lite.py` | High | 10-15 |
| `test_dynamic_info.py` | `bittensor/core/chain_data/dynamic_info.py` | High | 10-15 |
| `test_info_base.py` | `bittensor/core/chain_data/info_base.py` | High | 10-15 |
| `test_ip_info.py` | `bittensor/core/chain_data/ip_info.py` | High | 10-15 |
| `test_neuron_info.py` | `bittensor/core/chain_data/neuron_info.py` | Critical | 20-25 |
| `test_neuron_info_lite.py` | `bittensor/core/chain_data/neuron_info_lite.py` | High | 15-20 |
| `test_prometheus_info.py` | `bittensor/core/chain_data/prometheus_info.py` | Medium | 10-15 |
| `test_proposal_vote_data.py` | `bittensor/core/chain_data/proposal_vote_data.py` | Medium | 10-15 |
| `test_proxy.py` | `bittensor/core/chain_data/proxy.py` | Critical | 25-30 |
| `test_root_claim.py` | `bittensor/core/chain_data/root_claim.py` | High | 15-20 |
| `test_scheduled_coldkey_swap_info.py` | `bittensor/core/chain_data/scheduled_coldkey_swap_info.py` | Medium | 10-15 |
| `test_sim_swap.py` | `bittensor/core/chain_data/sim_swap.py` | Medium | 10-15 |
| `test_stake_info.py` | `bittensor/core/chain_data/stake_info.py` | Critical | 15-20 |
| `test_subnet_hyperparameters.py` | `bittensor/core/chain_data/subnet_hyperparameters.py` | High | 15-20 |
| `test_subnet_identity.py` | `bittensor/core/chain_data/subnet_identity.py` | High | 10-15 |
| `test_subnet_info.py` | `bittensor/core/chain_data/subnet_info.py` | Critical | 20-25 |
| `test_subnet_state.py` | `bittensor/core/chain_data/subnet_state.py` | High | 15-20 |
| `test_weight_commit_info.py` | `bittensor/core/chain_data/weight_commit_info.py` | High | 15-20 |

**Suggested test patterns for each chain data class:**

```python
# Example: test_delegate_info.py
class TestDelegateInfo:
    def test_init_with_valid_data(self):
        """Test initialization with valid parameters."""

    def test_init_with_none_values(self):
        """Test initialization handles None values appropriately."""

    def test_from_parameter_dict(self):
        """Test creating instance from chain parameter dict."""

    def test_from_parameter_dict_missing_fields(self):
        """Test handling missing fields in parameter dict."""

    def test_to_parameter_dict(self):
        """Test converting to parameter dict format."""

    def test_serialization_roundtrip(self):
        """Test serialize -> deserialize preserves data."""

    def test_equality_comparison(self):
        """Test __eq__ implementation."""

    def test_hash_implementation(self):
        """Test __hash__ for dict key usage."""

    def test_string_representation(self):
        """Test __str__ and __repr__ outputs."""

    def test_field_validation_invalid_types(self):
        """Test validation rejects invalid field types."""
```

---

### Critical Priority - Missing Extrinsics

**Location:** `tests/unit_tests/extrinsics/`

#### Synchronous Extrinsics (4 new test files):

| New Test File | Source Module | Lines | Priority |
|--------------|---------------|-------|----------|
| `test_mev_shield.py` | `bittensor/core/extrinsics/mev_shield.py` | 268 | Critical |
| `test_move_stake.py` | `bittensor/core/extrinsics/move_stake.py` | 515 | Critical |
| `test_sudo.py` | `bittensor/core/extrinsics/sudo.py` | 143 | High |
| `test_take.py` | `bittensor/core/extrinsics/take.py` | 89 | High |

**Suggested tests for `test_mev_shield.py`:**

```python
class TestMevShieldExtrinsics:
    def test_enable_mev_shield_success(self):
        """Test enabling MEV shield with valid parameters."""

    def test_enable_mev_shield_already_enabled(self):
        """Test enabling when already enabled."""

    def test_disable_mev_shield_success(self):
        """Test disabling MEV shield."""

    def test_disable_mev_shield_not_enabled(self):
        """Test disabling when not enabled."""

    def test_mev_shield_with_invalid_hotkey(self):
        """Test with non-existent hotkey."""

    def test_mev_shield_insufficient_balance(self):
        """Test with insufficient balance for fees."""

    def test_mev_shield_wait_for_inclusion(self):
        """Test wait_for_inclusion parameter."""

    def test_mev_shield_wait_for_finalization(self):
        """Test wait_for_finalization parameter."""
```

**Suggested tests for `test_move_stake.py`:**

```python
class TestMoveStakeExtrinsics:
    def test_move_stake_between_hotkeys_success(self):
        """Test moving stake between two hotkeys."""

    def test_move_stake_to_same_hotkey(self):
        """Test moving stake to same hotkey fails."""

    def test_move_stake_insufficient_stake(self):
        """Test moving more stake than available."""

    def test_move_stake_zero_amount(self):
        """Test moving zero stake."""

    def test_move_stake_negative_amount(self):
        """Test moving negative stake raises error."""

    def test_move_stake_invalid_source_hotkey(self):
        """Test with invalid source hotkey."""

    def test_move_stake_invalid_destination_hotkey(self):
        """Test with invalid destination hotkey."""

    def test_move_stake_batch_operations(self):
        """Test batch stake movements."""

    def test_move_stake_partial_amount(self):
        """Test moving partial stake amount."""

    def test_move_stake_full_amount(self):
        """Test moving all staked amount."""
```

#### Async Extrinsics (7 new test files):

**Location:** `tests/unit_tests/extrinsics/asyncex/`

| New Test File | Source Module | Priority |
|--------------|---------------|----------|
| `test_mev_shield.py` | `bittensor/core/extrinsics/asyncex/mev_shield.py` | Critical |
| `test_move_stake.py` | `bittensor/core/extrinsics/asyncex/move_stake.py` | Critical |
| `test_serving.py` | `bittensor/core/extrinsics/asyncex/serving.py` | Critical |
| `test_sudo.py` | `bittensor/core/extrinsics/asyncex/sudo.py` | High |
| `test_take.py` | `bittensor/core/extrinsics/asyncex/take.py` | High |
| `test_utils.py` | `bittensor/core/extrinsics/asyncex/utils.py` | High |

**Suggested async test patterns:**

```python
import pytest

class TestAsyncMevShieldExtrinsics:
    @pytest.mark.asyncio
    async def test_enable_mev_shield_async_success(self):
        """Test async MEV shield enable."""

    @pytest.mark.asyncio
    async def test_enable_mev_shield_async_timeout(self):
        """Test timeout handling in async operation."""

    @pytest.mark.asyncio
    async def test_concurrent_mev_shield_operations(self):
        """Test concurrent enable/disable operations."""
```

---

### High Priority - Pallet Operations

**Location:** `tests/unit_tests/extrinsics/pallets/` (NEW DIRECTORY)

| New Test File | Source Module | Priority |
|--------------|---------------|----------|
| `test_admin_utils.py` | `bittensor/core/extrinsics/pallets/admin_utils.py` | High |
| `test_balances.py` | `bittensor/core/extrinsics/pallets/balances.py` | High |
| `test_base.py` | `bittensor/core/extrinsics/pallets/base.py` | High |
| `test_commitments.py` | `bittensor/core/extrinsics/pallets/commitments.py` | High |
| `test_crowdloan.py` | `bittensor/core/extrinsics/pallets/crowdloan.py` | High |
| `test_mev_shield.py` | `bittensor/core/extrinsics/pallets/mev_shield.py` | High |
| `test_proxy.py` | `bittensor/core/extrinsics/pallets/proxy.py` | High |
| `test_subtensor_module.py` | `bittensor/core/extrinsics/pallets/subtensor_module.py` | Critical |
| `test_sudo.py` | `bittensor/core/extrinsics/pallets/sudo.py` | Medium |
| `test_swap.py` | `bittensor/core/extrinsics/pallets/swap.py` | Medium |

**Suggested pallet test patterns:**

```python
class TestSubtensorModulePallet:
    def test_pallet_name_constant(self):
        """Test pallet name is correctly defined."""

    def test_call_builder_valid_params(self):
        """Test call builder with valid parameters."""

    def test_call_builder_invalid_params(self):
        """Test call builder rejects invalid parameters."""

    def test_encode_call_data(self):
        """Test encoding call data for submission."""

    def test_decode_events(self):
        """Test decoding events from pallet."""
```

---

### High Priority - Extras Module

**Location:** `tests/unit_tests/extras/` (NEW DIRECTORY)

#### Subtensor API Tests:

| New Test File | Source Module | Priority |
|--------------|---------------|----------|
| `test_subtensor_api_init.py` | `bittensor/extras/subtensor_api/__init__.py` | High |
| `test_subtensor_api_metagraphs.py` | `bittensor/extras/subtensor_api/metagraphs.py` | High |
| `test_subtensor_api_subnets.py` | `bittensor/extras/subtensor_api/subnets.py` | High |
| `test_subtensor_api_delegates.py` | `bittensor/extras/subtensor_api/delegates.py` | High |
| `test_subtensor_api_wallets.py` | `bittensor/extras/subtensor_api/wallets.py` | High |
| `test_subtensor_api_queries.py` | `bittensor/extras/subtensor_api/queries.py` | High |
| `test_subtensor_api_staking.py` | `bittensor/extras/subtensor_api/staking.py` | High |
| `test_subtensor_api_neurons.py` | `bittensor/extras/subtensor_api/neurons.py` | High |
| `test_subtensor_api_crowdloans.py` | `bittensor/extras/subtensor_api/crowdloans.py` | Medium |
| `test_subtensor_api_proxy.py` | `bittensor/extras/subtensor_api/proxy.py` | Medium |
| `test_subtensor_api_chain.py` | `bittensor/extras/subtensor_api/chain.py` | Medium |
| `test_subtensor_api_mev_shield.py` | `bittensor/extras/subtensor_api/mev_shield.py` | Medium |
| `test_subtensor_api_extrinsics.py` | `bittensor/extras/subtensor_api/extrinsics.py` | Medium |
| `test_subtensor_api_commitments.py` | `bittensor/extras/subtensor_api/commitments.py` | Medium |
| `test_subtensor_api_utils.py` | `bittensor/extras/subtensor_api/utils.py` | Medium |

#### Dev Framework Tests:

| New Test File | Source Module | Priority |
|--------------|---------------|----------|
| `test_dev_framework_subnet.py` | `bittensor/extras/dev_framework/subnet.py` | High |
| `test_dev_framework_sudo_calls.py` | `bittensor/extras/dev_framework/calls/sudo_calls.py` | High |
| `test_dev_framework_non_sudo_calls.py` | `bittensor/extras/dev_framework/calls/non_sudo_calls.py` | High |
| `test_dev_framework_pallets.py` | `bittensor/extras/dev_framework/calls/pallets.py` | Medium |
| `test_dev_framework_utils.py` | `bittensor/extras/dev_framework/utils.py` | Medium |

#### Timelock Tests:

| New Test File | Source Module | Priority |
|--------------|---------------|----------|
| `test_timelock.py` | `bittensor/extras/timelock.py` | High |

---

### Medium Priority - Core Utilities

**Location:** `tests/unit_tests/`

| New Test File | Source Module | Lines | Priority |
|--------------|---------------|-------|----------|
| `test_threadpool.py` | `bittensor/core/threadpool.py` | 294 | High |
| `test_settings.py` | `bittensor/core/settings.py` | 167 | Medium |
| `test_types.py` | `bittensor/core/types.py` | 577 | Medium |
| `test_axon_utils.py` | `bittensor/utils/axon_utils.py` | ~50 | Medium |

**Suggested tests for `test_threadpool.py`:**

```python
class TestPriorityThreadPoolExecutor:
    def test_init_with_default_workers(self):
        """Test initialization with default worker count."""

    def test_init_with_custom_workers(self):
        """Test initialization with custom worker count."""

    def test_submit_single_task(self):
        """Test submitting single task."""

    def test_submit_multiple_tasks_priority_ordering(self):
        """Test tasks are executed in priority order."""

    def test_submit_same_priority_fifo(self):
        """Test same-priority tasks maintain FIFO order."""

    def test_shutdown_graceful(self):
        """Test graceful shutdown waits for tasks."""

    def test_shutdown_immediate(self):
        """Test immediate shutdown cancels pending tasks."""

    def test_max_workers_limit(self):
        """Test worker count doesn't exceed max."""

    def test_task_exception_handling(self):
        """Test exceptions in tasks are properly handled."""

    def test_concurrent_submit_thread_safety(self):
        """Test thread-safe concurrent task submission."""

    def test_future_result_retrieval(self):
        """Test retrieving results from futures."""

    def test_future_cancellation(self):
        """Test cancelling pending futures."""
```

**Suggested tests for `test_settings.py`:**

```python
class TestSettings:
    def test_default_values(self):
        """Test all default values are set correctly."""

    def test_network_constants(self):
        """Test network-related constants."""

    def test_timeout_defaults(self):
        """Test timeout default values."""

    def test_path_defaults(self):
        """Test default path configurations."""

    def test_version_info(self):
        """Test version information is accessible."""
```

---

## New Test Functions to Add to Existing Tests

### test_axon.py Additions

Add these test functions to `tests/unit_tests/test_axon.py`:

```python
# Lifecycle Tests
def test_axon_start_stop_lifecycle(self):
    """Test starting and stopping the axon server."""

def test_axon_start_multiple_times(self):
    """Test that starting an already-running axon handles state correctly."""

def test_axon_stop_when_not_running(self):
    """Test stopping an axon that isn't running."""

# Port/Network Tests
def test_serve_axon_with_invalid_port(self):
    """Test serve() with port 0 or negative numbers."""

def test_serve_axon_port_already_in_use(self):
    """Test error handling when port is already bound."""

def test_axon_start_with_invalid_ip(self):
    """Test handling of invalid IP addresses during start."""

# Attachment Tests
def test_attach_duplicate_synapse_types(self):
    """Test attaching multiple handlers for same Synapse type."""

def test_attach_async_forward_function(self):
    """Test async forward functions work correctly."""

def test_attach_priority_fn_none(self):
    """Test attach with None priority_fn uses default."""

def test_attach_verify_fn_none(self):
    """Test attach with None verify_fn uses default_verify."""

def test_attach_blacklist_fn_none(self):
    """Test attach with None blacklist_fn always allows."""

# Middleware Exception Tests
def test_middleware_exception_handling_in_forward(self):
    """Test exception handling when forward_fn raises."""

def test_middleware_exception_handling_in_blacklist(self):
    """Test exception handling when blacklist_fn raises."""

def test_middleware_exception_handling_in_priority(self):
    """Test exception handling when priority_fn raises."""

def test_middleware_exception_handling_in_verify(self):
    """Test exception handling when verify_fn raises."""

# Nonce Tests
def test_default_verify_with_invalid_nonce(self):
    """Test default_verify rejects too-old nonces."""

def test_default_verify_with_future_nonce(self):
    """Test default_verify handles future nonces."""

# Request Validation Tests
def test_axon_request_max_size_validation(self):
    """Test requests exceeding max size are rejected."""

def test_axon_invalid_json_request_body(self):
    """Test handling of malformed JSON in request body."""

def test_axon_missing_headers(self):
    """Test handling requests missing critical headers."""

def test_axon_invalid_header_values(self):
    """Test invalid header value types."""

def test_axon_signature_verification_failure(self):
    """Test request with invalid signature is rejected."""

# Response Tests
def test_axon_response_content_type_header(self):
    """Test responses include correct content-type headers."""

def test_axon_response_custom_headers(self):
    """Test custom headers are preserved in responses."""

def test_axon_version_in_response(self):
    """Test axon version is set in response headers."""

# Concurrency Tests
def test_axon_concurrent_requests_ordering(self):
    """Test priority function correctly orders concurrent requests."""

def test_axon_priority_timeout_requests(self):
    """Test handling of requests with timeout priority."""
```

---

### test_dendrite.py Additions

Add these test functions to `tests/unit_tests/test_dendrite.py`:

```python
# Query Tests
def test_dendrite_query_single_axon_with_custom_timeout(self):
    """Test query with custom timeout values."""

def test_dendrite_query_multiple_axons_concurrent(self):
    """Test concurrent queries to multiple axons."""

def test_dendrite_query_multiple_axons_sequential(self):
    """Test sequential queries (run_async=False)."""

def test_dendrite_query_empty_axon_list(self):
    """Test querying with empty axon list."""

def test_dendrite_query_with_none_axon(self):
    """Test querying with None as axon."""

def test_dendrite_query_with_custom_synapse_subclass(self):
    """Test querying with custom synapse types."""

# Deserialization Tests
def test_dendrite_call_with_deserialization(self):
    """Test call() with deserialize=True."""

def test_dendrite_call_with_deserialization_disabled(self):
    """Test call() with deserialize=False."""

# Streaming Tests
def test_dendrite_streaming_response_handling(self):
    """Test handling of streaming responses."""

def test_dendrite_streaming_chunked_data(self):
    """Test streaming with multiple chunks."""

def test_dendrite_streaming_generator_consumption(self):
    """Test async generator iteration."""

# Session Management Tests
def test_dendrite_session_cleanup_on_error(self):
    """Test session cleanup when query fails."""

def test_dendrite_session_reuse_across_queries(self):
    """Test sessions can be reused across queries."""

def test_dendrite_close_session_already_closed(self):
    """Test closing an already closed session."""

def test_dendrite_context_manager_usage(self):
    """Test async context manager initialization."""

# Network Error Tests
def test_dendrite_connection_timeout_handling(self):
    """Test timeout during connection establishment."""

def test_dendrite_ssl_certificate_error(self):
    """Test SSL/TLS certificate validation errors."""

def test_dendrite_ssl_hostname_mismatch(self):
    """Test SSL hostname mismatch error handling."""

def test_dendrite_dns_resolution_error(self):
    """Test handling DNS resolution failures."""

def test_dendrite_network_error_recovery(self):
    """Test retry logic on temporary network errors."""

# Response Handling Tests
def test_dendrite_invalid_response_format(self):
    """Test handling non-JSON response from axon."""

def test_dendrite_partial_response_handling(self):
    """Test handling incomplete responses."""

def test_dendrite_large_response_handling(self):
    """Test handling large response payloads."""

# Preprocessing Tests
def test_dendrite_preprocess_synapse_nonce_generation(self):
    """Test nonce is generated and unique."""

def test_dendrite_preprocess_synapse_signature_generation(self):
    """Test dendrite signature generation."""

def test_dendrite_preprocess_synapse_version_set(self):
    """Test dendrite version is set during preprocessing."""

# Return Type Tests
def test_dendrite_forward_return_type_single_axon(self):
    """Test forward returns single Synapse for single axon."""

def test_dendrite_forward_return_type_multiple_axons(self):
    """Test forward returns list for multiple axons."""
```

---

### test_metagraph.py Additions

Add these test functions to `tests/unit_tests/test_metagraph.py`:

```python
# Sync Tests
def test_metagraph_sync_with_valid_netuid(self):
    """Test sync with valid subnet ID."""

def test_metagraph_sync_with_invalid_netuid(self):
    """Test sync fails with invalid subnet ID."""

def test_metagraph_sync_with_current_block(self):
    """Test sync with current block number."""

def test_metagraph_sync_with_past_block(self):
    """Test sync with block from past."""

def test_metagraph_sync_lite_mode(self):
    """Test sync with lite=True."""

def test_metagraph_sync_full_mode(self):
    """Test sync with lite=False."""

def test_metagraph_sync_without_subtensor(self):
    """Test sync fails when subtensor is None."""

def test_metagraph_sync_with_empty_subnet(self):
    """Test sync on subnet with no neurons."""

def test_metagraph_sync_with_single_neuron(self):
    """Test sync on subnet with one neuron."""

def test_metagraph_sync_with_large_neuron_count(self):
    """Test sync with many neurons (stress test)."""

# Neuron Filtering Tests
def test_metagraph_neuron_filtering_by_uid(self):
    """Test filtering neurons by UID."""

def test_metagraph_neuron_filtering_by_hotkey(self):
    """Test filtering neurons by hotkey."""

# Calculation Tests
def test_metagraph_stake_aggregation(self):
    """Test total_stake is correctly calculated."""

def test_metagraph_emission_calculation(self):
    """Test emission values are preserved."""

def test_metagraph_incentive_calculation(self):
    """Test incentive values are preserved."""

def test_metagraph_rank_calculation(self):
    """Test rank values are correctly set."""

def test_metagraph_trust_calculation(self):
    """Test trust values are correctly set."""

# Persistence Tests
def test_metagraph_save_to_disk(self):
    """Test save() creates correct files."""

def test_metagraph_load_from_disk(self):
    """Test load() restores metagraph state."""

def test_metagraph_save_and_load_roundtrip(self):
    """Test save->load preserves all data."""

def test_metagraph_load_from_invalid_path(self):
    """Test load with non-existent directory."""

def test_metagraph_load_from_corrupted_file(self):
    """Test load with corrupted saved state."""

# Property Tests
def test_metagraph_metadata_property(self):
    """Test metadata() returns correct info."""

def test_metagraph_state_dict(self):
    """Test state_dict() returns all attributes."""

def test_metagraph_hotkeys_property(self):
    """Test hotkeys property returns correct list."""

def test_metagraph_coldkeys_property(self):
    """Test coldkeys property returns correct list."""

def test_metagraph_axon_access(self):
    """Test accessing axons from metagraph."""

# Comparison Tests
def test_metagraph_comparison_equal(self):
    """Test two metagraphs with same data are equal."""

def test_metagraph_comparison_different(self):
    """Test two metagraphs with different data are not equal."""

# Access Tests
def test_metagraph_neuron_access_by_index(self):
    """Test accessing neurons by index."""
```

---

### test_subtensor.py Additions

Add these test functions to `tests/unit_tests/test_subtensor.py`:

```python
# Connection Tests
def test_connect_to_invalid_endpoint(self):
    """Test connecting to non-existent endpoint."""

def test_connect_with_network_timeout(self):
    """Test connection timeout handling."""

# Hyperparameter Tests
def test_hyperparameter_with_missing_subnet(self):
    """Test querying hyperparameter for non-existent subnet."""

def test_hyperparameter_with_invalid_type_conversion(self):
    """Test hyperparameter value type conversion failure."""

# Query Tests
def test_query_with_connection_error(self):
    """Test query fails gracefully with connection error."""

def test_query_map_with_large_dataset(self):
    """Test querying large maps doesn't cause memory issues."""

def test_metagraph_sync_with_network_error(self):
    """Test metagraph sync handles network errors."""

def test_get_current_block_with_network_error(self):
    """Test block query with network error."""

# Hotkey Tests
def test_is_hotkey_registered_with_invalid_hotkey_format(self):
    """Test with malformed hotkey string."""

def test_is_hotkey_registered_with_empty_hotkey(self):
    """Test with empty hotkey string."""

def test_get_uid_for_hotkey_not_found(self):
    """Test returns None when hotkey not found."""

def test_get_uid_for_hotkey_multiple_registrations(self):
    """Test with hotkey on multiple subnets."""

# Staking Tests
def test_add_stake_insufficient_balance(self):
    """Test stake with insufficient balance."""

def test_add_stake_with_zero_amount(self):
    """Test stake with 0 amount."""

def test_add_stake_with_negative_amount(self):
    """Test stake with negative amount."""

def test_unstake_more_than_staked(self):
    """Test unstaking more than staked amount."""

def test_unstake_with_zero_amount(self):
    """Test unstake with 0 amount."""

def test_unstake_with_negative_amount(self):
    """Test unstake with negative amount."""

def test_swap_stake_invalid_hotkey(self):
    """Test swap with non-existent hotkey."""

def test_swap_stake_insufficient_balance(self):
    """Test swap with insufficient balance."""

def test_stake_operations_concurrent(self):
    """Test concurrent stake operations."""

# Weight Tests
def test_commit_weights_on_rate_limited_subnet(self):
    """Test commit fails when rate limited."""

def test_reveal_weights_without_previous_commit(self):
    """Test reveal fails without commit."""

def test_reveal_weights_with_invalid_nonce(self):
    """Test reveal fails with mismatched nonce."""

def test_weights_rate_limit_boundary(self):
    """Test at exact rate limit boundary."""

def test_weights_rate_limit_zero_timeout(self):
    """Test with rate limit timeout of 0."""

# Neuron Tests
def test_neurons_with_high_uid(self):
    """Test neuron queries with max UID."""

def test_neurons_with_negative_uid(self):
    """Test neuron queries with negative UID."""

# Transfer Tests
def test_transfer_to_invalid_destination(self):
    """Test transfer to invalid address format."""

def test_transfer_zero_amount(self):
    """Test transfer with 0 TAO."""

def test_transfer_insufficient_balance(self):
    """Test transfer with insufficient balance."""

# Delegate Tests
def test_get_delegate_take_invalid_hotkey(self):
    """Test delegate take with non-existent hotkey."""

def test_get_minimum_required_stake_zero_result(self):
    """Test minimum stake when result is 0."""

# Edge Cases
def test_subnet_exists_at_boundary_block(self):
    """Test subnet existence at specific blocks."""

def test_blockchain_reorganization_handling(self):
    """Test handling of chain reorg events."""
```

---

### test_stream.py Additions

Add these test functions to `tests/unit_tests/test_stream.py`:

```python
# Payload Tests
def test_streaming_response_with_very_large_payload(self):
    """Test streaming with multi-MB payloads."""

def test_streaming_response_empty_stream(self):
    """Test streaming with no data chunks."""

def test_streaming_response_single_large_chunk(self):
    """Test streaming with one very large chunk."""

def test_streaming_response_many_small_chunks(self):
    """Test streaming with many small chunks."""

# Client Behavior Tests
def test_streaming_response_with_slow_client(self):
    """Test streaming with slow reader causing backpressure."""

def test_streaming_response_client_disconnect_during_stream(self):
    """Test handling client disconnection mid-stream."""

def test_streaming_response_slow_streamer(self):
    """Test with token_streamer that takes time between sends."""

# Concurrency Tests
def test_streaming_response_concurrent_streams(self):
    """Test multiple concurrent streaming responses."""

# Header Tests
def test_streaming_response_custom_headers_preservation(self):
    """Test custom headers are included in stream."""

def test_streaming_response_with_custom_status_code(self):
    """Test streaming response with non-200 status."""

def test_streaming_response_with_custom_content_type(self):
    """Test custom content-type headers."""

# Error Handling Tests
def test_streaming_response_timeout_handling(self):
    """Test timeout during streaming."""

def test_streaming_response_network_interruption(self):
    """Test handling network interruption during stream."""

def test_streaming_response_exception_in_streamer_cleanup(self):
    """Test cleanup when streamer raises during cleanup."""

def test_streaming_response_generator_early_termination(self):
    """Test stopping generator before completion."""

# Memory Tests
def test_streaming_response_memory_efficiency_large_stream(self):
    """Test memory doesn't grow linearly with stream size."""

# Data Processing Tests
def test_process_streaming_response_with_binary_chunks(self):
    """Test processing binary data chunks."""

def test_process_streaming_response_with_text_chunks(self):
    """Test processing text data chunks."""

def test_process_streaming_response_with_mixed_chunks(self):
    """Test processing mixed binary/text chunks."""

# JSON Extraction Tests
def test_extract_response_json_with_empty_response(self):
    """Test JSON extraction from empty response."""

def test_extract_response_json_with_large_json(self):
    """Test extracting large JSON payloads."""

def test_extract_response_json_with_invalid_json(self):
    """Test extraction when response contains invalid JSON."""

# Creation Tests
def test_create_streaming_response_multiple_invocations(self):
    """Test creating multiple responses from same synapse."""

def test_streaming_synapse_subclass_inheritance_chain(self):
    """Test multi-level inheritance of StreamingSynapse."""

# ASGI Tests
def test_streaming_response_asgi_scope_variations(self):
    """Test with different ASGI scope configurations."""
```

---

### test_synapse.py Additions

Add these test functions to `tests/unit_tests/test_synapse.py`:

```python
# Header Parsing Tests
def test_parse_headers_with_missing_timeout(self):
    """Test header parsing when timeout is missing."""

def test_parse_headers_with_invalid_timeout_format(self):
    """Test with non-numeric timeout."""

def test_parse_headers_with_negative_timeout(self):
    """Test parsing negative timeout."""

def test_parse_headers_with_very_large_timeout(self):
    """Test with extremely large timeout."""

def test_parse_headers_with_special_characters_in_name(self):
    """Test synapse name with special chars."""

def test_parse_headers_with_missing_required_fields(self):
    """Test missing required fields fail."""

def test_parse_headers_with_extra_unknown_headers(self):
    """Test unknown headers are ignored."""

# Type Coercion Tests
def test_from_headers_type_coercion_string_to_int(self):
    """Test automatic type coercion string to int."""

def test_from_headers_type_coercion_string_to_float(self):
    """Test automatic type coercion string to float."""

def test_from_headers_type_coercion_string_to_bool(self):
    """Test automatic type coercion string to bool."""

def test_from_headers_invalid_type_coercion(self):
    """Test type coercion failure handling."""

# Base64 Tests
def test_from_headers_base64_encoded_list(self):
    """Test base64 decoding of list fields."""

def test_from_headers_base64_invalid_encoding(self):
    """Test invalid base64 in headers."""

# Header Conversion Tests
def test_to_headers_roundtrip(self):
    """Test synapse->headers->synapse preserves data."""

def test_to_headers_with_none_values(self):
    """Test header conversion with None fields."""

def test_to_headers_large_list_field(self):
    """Test converting large lists to headers."""

def test_to_headers_circular_reference(self):
    """Test circular references don't cause infinite loops."""

# Initialization Tests
def test_synapse_initialization_with_invalid_timeout(self):
    """Test creating synapse with invalid timeout."""

def test_synapse_initialization_with_negative_sizes(self):
    """Test creating with negative header_size or total_size."""

def test_synapse_initialization_with_extremely_large_sizes(self):
    """Test with size values > available memory."""

# Body Hash Tests
def test_synapse_body_hash_with_empty_fields(self):
    """Test body hash with all empty hash fields."""

def test_synapse_body_hash_consistency_across_instances(self):
    """Test two instances with same data have same hash."""

def test_synapse_body_hash_immutability(self):
    """Test hash doesn't change after field modification."""

def test_synapse_required_hash_fields_with_missing_field(self):
    """Test hash calculation when required field is missing."""

def test_synapse_required_hash_fields_override_attempt(self):
    """Test overriding required_hash_fields doesn't work."""

# Serialization Tests
def test_synapse_custom_field_in_header_serialization(self):
    """Test custom synapse fields are serialized to headers."""

def test_synapse_custom_field_type_preservation(self):
    """Test field types are preserved through serialization."""

def test_synapse_nested_object_serialization(self):
    """Test nested objects serialize correctly."""

# Info Tests
def test_synapse_axon_info_in_headers(self):
    """Test axon info is properly encoded/decoded."""

def test_synapse_dendrite_info_in_headers(self):
    """Test dendrite info is properly encoded/decoded."""

# Field Validation Tests
def test_synapse_signature_field_validation(self):
    """Test signature field format validation."""

def test_synapse_nonce_field_validation(self):
    """Test nonce field format validation."""

def test_synapse_uuid_field_validation(self):
    """Test UUID field format validation."""

def test_synapse_version_field_range(self):
    """Test version field accepts valid range."""

def test_synapse_status_code_out_of_range(self):
    """Test status code validation."""
```

---

## Test Coverage Statistics

### Current State

| Category | Test Files | Test Functions | Lines of Test Code |
|----------|------------|----------------|-------------------|
| Unit Tests | 51 | ~500 | ~17,741 |
| E2E Tests | 26 | ~300 | ~15,238 |
| Integration Tests | 5 | ~50 | ~3,000 |
| Consistency Tests | 1 | ~10 | ~500 |
| **Total** | **83** | **~860** | **~36,479** |

### After Implementing Improvements

| Category | Test Files | Test Functions | Estimated Lines |
|----------|------------|----------------|-----------------|
| Unit Tests | 98 (+47) | ~1,200 (+700) | ~35,000 |
| E2E Tests | 26 | ~300 | ~15,238 |
| Integration Tests | 5 | ~50 | ~3,000 |
| Consistency Tests | 1 | ~10 | ~500 |
| **Total** | **130** | **~1,560** | **~53,738** |

---

## Implementation Priority Matrix

### Phase 1 - Critical (Week 1-2)

| Priority | Task | Files to Create | Est. Tests |
|----------|------|-----------------|------------|
| P0 | Chain data module tests | 21 new files | 350+ |
| P0 | Missing sync extrinsic tests | 4 new files | 60+ |
| P0 | Missing async extrinsic tests | 7 new files | 80+ |

### Phase 2 - High (Week 3-4)

| Priority | Task | Files to Create | Est. Tests |
|----------|------|-----------------|------------|
| P1 | Pallet operation tests | 10 new files | 100+ |
| P1 | Extras/subtensor_api tests | 15 new files | 150+ |
| P1 | Add functions to test_axon.py | 0 (modify) | 30+ |
| P1 | Add functions to test_dendrite.py | 0 (modify) | 30+ |

### Phase 3 - Medium (Week 5-6)

| Priority | Task | Files to Create | Est. Tests |
|----------|------|-----------------|------------|
| P2 | Core utility tests | 4 new files | 50+ |
| P2 | Dev framework tests | 5 new files | 60+ |
| P2 | Add functions to test_metagraph.py | 0 (modify) | 30+ |
| P2 | Add functions to test_subtensor.py | 0 (modify) | 35+ |
| P2 | Add functions to test_stream.py | 0 (modify) | 25+ |
| P2 | Add functions to test_synapse.py | 0 (modify) | 35+ |

---

## Summary

This document identifies **92+ new test files** and **185+ new test functions** to improve SDKv10 test coverage:

**New Test Scripts (92 files):**
- Chain Data Module: 21 files
- Sync Extrinsics: 4 files
- Async Extrinsics: 7 files
- Pallet Operations: 10 files
- Extras/Subtensor API: 15 files
- Dev Framework: 5 files
- Core Utilities: 4 files

**New Test Functions for Existing Files (185+ functions):**
- test_axon.py: 30 functions
- test_dendrite.py: 30 functions
- test_metagraph.py: 30 functions
- test_subtensor.py: 35 functions
- test_stream.py: 25 functions
- test_synapse.py: 35 functions

**Focus Areas:**
1. Network error handling and resilience
2. Edge cases and boundary conditions
3. Concurrent operations
4. Invalid input validation
5. Serialization/deserialization roundtrips
6. Resource cleanup and lifecycle management
