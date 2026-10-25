---
title: '[MCP Gateway] Move cost tracking and permission management to OSS by jugaldb
  · Pull Request #12988 · BerriAI/litellm'
id: mcp-gateway-move-cost-tracking-and-permission-management-to-oss-by-jugaldb-pull
tags:
- internal-llm-inference-stacks-7f0c90
- locus-oss-boundary-sharpening
- litellm
- open-core
created: '2026-10-10T00:07:56.879564Z'
updated: '2026-10-10T14:52:17.268224Z'
source: https://github.com/BerriAI/litellm/pull/12988
source_domain: github.com
fetched_at: '2026-10-10T00:07:56.856449Z'
fetch_provider: crawl4ai
status: review
type: note
tier: ground_truth
content_type: code
deprecated: false
summary: 'LiteLLM PR #12988: moves MCP gateway cost tracking and permission management
  from Enterprise into open source — the counter-example showing the project does
  sometimes open-source commercial features, which is what makes the closed tag-budget
  request a specific choice.'
---

[Skip to content](https://github.com/BerriAI/litellm/pull/12988#start-of-content)
You signed in with another tab or window. [Reload](https://github.com/BerriAI/litellm/pull/12988) to refresh your session. You signed out in another tab or window. [Reload](https://github.com/BerriAI/litellm/pull/12988) to refresh your session. You switched accounts on another tab or window. [Reload](https://github.com/BerriAI/litellm/pull/12988) to refresh your session. Dismiss alert
## Conversation
### 
commented [Jul 25, 2025](https://github.com/BerriAI/litellm/pull/12988#issue-3263941743)
Copy link  Copy Markdown 
Contributor
[MCP Gateway] Move cost tracking and permission management to OSS
## Relevant issues
Fixes LIT-366
## Pre-Submission checklist
**Please complete all items before asking a LiteLLM maintainer to review your PR**
  * I have Added testing in the [`tests/litellm/`](https://github.com/BerriAI/litellm/tree/main/tests/litellm) directory, **Adding at least 1 test is a hard requirement** - [see details](https://docs.litellm.ai/docs/extras/contributing_code)
  * I have added a screenshot of my new test passing locally
  * My PR passes all unit tests on [`make test-unit`](https://docs.litellm.ai/docs/extras/contributing_code)
  * My PR's scope is as isolated as possible, it only solves 1 specific problem


## Type
🆕 New Feature
## Changes
` remove features from enterprise[](https://github.com/BerriAI/litellm/pull/12988/commits/c76975dc8b8bf98e857844632a6637f722257a0e "remove features from enterprise") `
### 
commented [Jul 25, 2025](https://github.com/BerriAI/litellm/pull/12988#issuecomment-3119718561)
Copy link  Copy Markdown   
|  **The latest updates on your projects**. Learn more about [Vercel for Git ↗︎](https://vercel.link/github-learn-more)  
 | Name  | Status  | Preview  | Comments  | Updated (UTC)  |  
| --- | --- | --- | --- | --- |  
| **litellm**  | ✅ Ready ([Inspect](https://vercel.com/clerkieai/litellm/73G5VJrt2xeY5qS93t6n1foNvM57))  | Jul 25, 2025 5:43pm  |  
 |  
**ghost** merged commit [`d1b6356`](https://github.com/BerriAI/litellm/commit/d1b63566accb690f070e949ba6f513fc0f38ea17) into [Jul 25, 2025](https://github.com/BerriAI/litellm/pull/12988#event-18819055615)
[dodoman-sun](https://github.com/dodoman-sun) referenced this pull request in dodoman-sun/litellm [ Aug 5, 2025 ](https://github.com/BerriAI/litellm/pull/12988#ref-commit-7f58f91)
Partially verified
#  Partially verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
We cannot verify signatures from co-authors, and some of the co-authors attributed to this commit require their commits to be signed. 

```
* Revert "EditAutoRouterTabProps"

This reverts commit 2835d3a3743e6411b9914a0b01381050e2273ad7.

* ui new build

* bump: version 1.74.8 → 1.74.9

* [Feat] Add inpainting support and corresponding tests for Amazon Nova Canvas (#12949)

* Added documentation about metadata exposed over the /v1/models endpoint (#12942)

* Fix: Shorten Gemini tool_call_id for Azure compatibility (#12941)

* feat: Update model pricing and context window configurations (#12910)

- Adjusted input and output cost per token for existing models.
- Added new model configuration for "openrouter/qwen/qwen3-coder" with specified token limits and costs.

* fix(auth_utils): make header comparison case-insensitive (#12950)

If the user specified in the configuration e.g. "user_header_name:
X-OpenWebUI-User-Email", here we were looking for a dict key
"X-OpenWebUI-User-Email" when the dict actually contained
"x-openwebui-user-email".

Switch to iteration and case insensitive string comparison instead to
fix this.

This fixes customer budget enforcement when the customer ID is passed
in as a header rather than as a "user" value in the body.

* GuardrailsAI: use validatedOutput to allow usage of "fix" guards. Previously "fix" guards had no effect in llmOutput mode. (#12891)

* Show global retry policy on UI  (#12969)

* fix(router.py): return global retry policy on `get/config/callbacks`

Partial fix for https://github.com/BerriAI/litellm/issues/12855

* fix(model_dashboard.tsx): accept global retry policy

Fixes https://github.com/BerriAI/litellm/issues/12855

* fix(model_dashboard.tsx): update global retry policy, if that's what was edited

* Guardrails - support model-level guardrails  (#12968)

* fix(custom_guardrail.py): initial logic for model level guardrails

* feat(custom_guardrail.py): working pre call guardrails

* fix(custom_guardrails.py): check if custom guardrails set before running event hook

* test(test_custom_guardrail.py): add unit tests for async pre call deployment hook on custom guardrail

* feat(custom_guardrail.py): add post call processing support for guardrails

allows model based guardrails to run on the post call event for that model only

* fix(utils.py): only run if call type is in enum

* test: update unit tests to work

* docs Health Check Server

* docs update

* docs update

* fix mapped test

* docs - auto routing

* docs auto routing

* docs - auto router on litellm proxy

* docs auto router

* fix ci/cd testing

* docs fix link

* build(github/manual_pypi_publish.yml): manual workflow to publish pip package - used for pushing dev releases (#12985)

* build(github/manual_pypi_publish.yml): manual workflow to publish pip package - used for pushing dev releases

* ci: remove redundant file

* [LLM Translation] Add bytedance/ui-tars-1.5-7b on openrouter (#12882)

* add bytedance model

* add source

* clean and verify key before inserting (#12840)

* clean and verify key

* change checking logic

* Add unit test

* [LLM Translation] fix query params for realtime api intent (#12838)

* fix query params for realtime api intent

* fix my py

* Add typed dict

* remove typed dict

* fix comments

* add test

* add test

* added proxt log revert

* add real time q params

* remove features from enterprise (#12988)

* feat(proxy/utils.py): support model level guardrails on stream event

enables guardrails to work with streaming

* feat(proxy_server.py): support checking full str on streaming guardrails post call hook

ensures streaming guardrails are actually useful

* build: update pip package (#12998)

* Fix issue writing db (#13001)

* add fix for redaction (#13005)

* [MCP Gateway] add Litellm mcp alias for prefixing (#12994)

* change alias-> server_name

* add server alias uses

* add tests

* schema

* ruff fix

* fix alias for config

* fix tests

* add alias

* fix tests

* fix tests

* add a common util

* ruff fix

* fix migration

* Fixup ollama model listing (again) (#13008)

* [Vector Store] make vector store permission management OSS (#12990)

* add vector store on ui behind enterprise in vector store

* remove enterprise

* [FEAT] Model-Guardrails: Add on UI (#13006)

* feat(proxy_server.py): working guardrails on streaming output

ensures guardrail actually raises an error if flagged during streaming output

* test: add unit tests

* feat(advanced_settings.tsx): add guardrails option as ui component on model add

enables setting guardrails on model add

* feat(add_model_tab.tsx): fix add model form

* feat(model_info_view.tsx): support adding guardrails on model update

* fix(add_model_tab.tsx/): working health check when guardrails selected

* fix(proxy_server.py): fix yield

* UI SSO - fix reset env var when ui_access_mode is updated  (#13011)

* fix(ui_sso.py): fix form action on login when sso is enabled

* fix: multiple fixes - fix resetting env var in proxy config + add key to exception message on key decryption

fixes issue where env vars would be reset

* refactor(proxy_server.py): cleanup redundant decryption line

* fix(proxy_setting_endpoints.py): show saved ui access mode

allows admin to know what they'd previously stored in db

* [MCP Gateway] Litellm mcp multi header propagation (#13003)

* change alias-> server_name

* add server alias uses

* add tests

* schema

* ruff fix

* fix alias for config

* fix tests

* add alias

* fix tests

* add multi server header support

* add and fix tests

* fix tests

* fix tests

* add a common util

* ruff fix

* fix ruff

* fix tests

* fix migration

* mypy fix

* change server py

* test_router_auto_router

* Litellm release notes 07 27 2025 p1 (#13027)

* docs(index.md): initial commit for v1.74.9-stable release note

* docs(index.md): add more cost tracking models

* docs(index.md): add new llm api endpoints + mcp gateway features

* docs: add logging/guardrail improvements

* docs(index.md): complete initial draft

* build(model_prices_and_context_window.json): fix or pricing

* build(model_prices_and_context_window.json): fix or pricing

* test: fix test

* VertexAI - camelcase optional params for image generation + Anthropic - streaming, always ensure assistant role set on only first chunk (#12889)

* fix(vertex_ai/image_generation): transform `_` param to camelcase

Fixes https://github.com/BerriAI/litellm/issues/12690

* test(test_vertex_image_generation.py): add unit tests

* fix(streaming_handler.py): assert only 1 assistant chunk in stream

Fixes https://github.com/BerriAI/litellm/issues/12616

* fix(streaming_handler.py): fix check

* Bulk User Edit - additional improvements - edit all users + set 'no-default-models' on all users (#12925)

* feat(bulk_user_update/): support updating all users on proxy

* fix(bulk_edit_user.tsx): persist user settings when 'add to team' clicked

* fix(team_endpoints.py): bulk add all proxy users to team

supports flow from UI to add all existing users to a team

* fix: minor fixes

* feat(user_edit_view.tsx): support setting no default model on user edit

allows preventing users from calling models outside team scope

* fix(user_edit_view.tsx): prevent triggering submit when 'cancel' is clicked

* refactor(internal_user_endpoints.py): refactor to reduce function size

* build: build new ui

* fix(proxy_settings_endpoints.py): fix clearing SSO settings

* refactor(create_key_button.tsx): cleanup read only option (confusing)

* build: update ui build

* test: update logic to fix for unit tests

* fix: add X-Initiator header for GitHub Copilot to reduce premium requests (#13016)

- Implement X-Initiator header logic in GithubCopilotConfig.validate_environment()
- Set header to "agent" when messages contain agent or tool roles, "user" otherwise
- Reduces unnecessary premium Copilot API usage for non-user calls

Fixes #12859

* docs - openweb show how to include reasoning content (#13060)

* build: bump pip

* [Bug Fix] Pass through logging handler VertexAI - ensure multimodal embedding responses are logged  (#13050)

* fix _is_multimodal_embedding_response

* test_vertex_passthrough_handler_multimodal_embedding_response

* Remove duplicate test case verifying field filtering logic (#13023)

Signed-off-by: Emmanuel Ferdman <emmanuelferdman@gmail.com>

* Properly parse json options for key generation in the UI (#12989)

* fix: correct CompletionRequest messages type to match OpenAI API spec (#12980)

* fix: correct CompletionRequest messages type to match OpenAI API spec

- Changed messages field type from List[str] to List[ChatCompletionMessageParam]
- This ensures proper OpenAI API compatibility where messages should be objects with role and content fields
- Fixes type inconsistency in completion request handling

* feat(tests): Add comprehensive tests for CompletionRequest model

- Add test_completion.py for litellm.types.completion module
- Test ChatCompletionMessageParam type validation
- Test tool message format compatibility
- Test function message format (deprecated)
- Test multimodal content (text + image)
- Test default empty messages list
- Test all optional parameters
- Validate OpenAI ChatCompletion API message format compatibility

* chore: Improve docs for cost tracking (#12976)

* feat(langfuse-otel): Add comprehensive metadata support to Langfuse OpenTelemetry integration (#12956)

* feat(langfuse-otel): Add comprehensive metadata support to Langfuse OpenTelemetry integration

This commit brings the langfuse_otel integration to feature parity with the vanilla Langfuse integration by adding support for all metadata fields.

Changes:
- Extended LangfuseSpanAttributes enum with all supported metadata fields:
  - Generation-level: generation_name, generation_id, parent_observation_id, version, mask_input/output
  - Trace-level: trace_user_id, session_id, tags, trace_name, trace_id, trace_metadata, trace_version, trace_release, existing_trace_id, update_trace_keys
  - Debug: debug_langfuse

- Implemented metadata extraction and mapping in langfuse_otel.py:
  - Added _extract_langfuse_metadata() helper to extract metadata from kwargs
  - Support for header-based metadata (langfuse_* headers) via proxy
  - Enhanced _set_langfuse_specific_attributes() to map all metadata to OTEL attributes
  - JSON serialization for complex types (lists, dicts) for OTEL compatibility

- Updated documentation:
  - Added 'Metadata Support' section explaining all fields are now supported
  - Provided usage example showing how to pass metadata
  - Clarified that traces are viewed in Langfuse UI (not generic OTEL backends)
  - Added opentelemetry-exporter-otlp to required dependencies

This allows users to pass metadata like:
metadata={
    'generation_name': 'my-generation',
    'trace_id': 'trace-123',
    'session_id': 'session-456',
    'tags': ['prod', 'v1'],
    'trace_metadata': {'user_type': 'premium'}
}

All metadata is exported as OpenTelemetry span attributes with 'langfuse.*' prefix for easy filtering and analysis in the Langfuse UI.

* Fix ruff linting error

* test(langfuse-otel): Fix failing test and add comprehensive metadata tests

- Fix test_set_langfuse_environment_attribute to use positional arguments
  instead of keyword arguments when asserting safe_set_attribute calls
- Add test_extract_langfuse_metadata_basic to verify metadata extraction
  from litellm_params
- Add test_extract_langfuse_metadata_with_header_enrichment to test
  integration with header-based metadata using a stubbed LangFuseLogger
- Add test_set_langfuse_specific_attributes_full_mapping to comprehensively
  test all metadata field mappings and JSON serialization of complex types

These tests ensure full coverage of the langfuse_otel metadata features
added in commit ab1dbe355 and fix the CI test failure.

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>

---------

Co-authored-by: Claude <noreply@anthropic.com>

* fix scrolling issue (#13015)

* [MCP gateway] add url namespacing docs (#13063)

* added the url docs

* Added url change

* test: skip dbrx claude 3-7 sonnet test - rate limit errors

* [Bug Fix] The model gemini-2.5-flash with the merge_reasoning_content_in_choices parameter does not work (#13066)

* _optional_combine_thinking_block_in_choices

* test_optional_combine_thinking_block_with_none_content

* test: remove o1-preview

* bump: version 1.74.9 → 1.74.10

* [Feat] Add Google AI Studio Imagen4 model family  (#13065)

* add gemini

* add init files

* add get_gemini_image_generation_config

* refactor transform

* TestGoogleImageGen

* fix transform

* fix transform

* add gemini_image_cost_calculator

* add cost tracking for gemini/imagen models

* docs image gen

* docs image gen

* test_get_model_info_gemini

* default to 7 days (#12917)

* Added handling for pwd protected cert files in AOAI CertificateCredential auth (#12995)

* docs: add Qwen Code CLI tutorial (#12915)

- Add new tutorial for integrating Qwen Code CLI with LiteLLM Proxy
- Update sidebar to include Qwen Code CLI in both AI Tools and main Tutorials sections
- Document environment variables for OpenAI-compatible configuration
- Include examples for routing to various providers (Anthropic, OpenAI, Bedrock)

* docs

* Azure `api_version="preview"` support + Bedrock cost tracking via Anthropic `/v1/messages`  (#13072)

* fix(azure/chat/gpt_transformation.py): support api_version="preview"

Fixes https://github.com/BerriAI/litellm/issues/12945

* Fix anthropic passthrough logging handler model fallback for streaming requests (#13022)

* fix: anthropic passthrough logging handler model fallback for streaming requests

- Add fallback logic to retrieve model from logging_obj.model_call_details when request_body.model is empty
- Fixes issue #12933 where streaming requests to anthropic passthrough endpoints would crash due to missing model field
- Ensures downstream logging and cost calculation work correctly for all streaming scenarios
- Maintains backwards compatibility with existing non-streaming requests

* test: add minimal tests for anthropic passthrough logging handler model fallback

- Add unit tests for the model fallback logic in _handle_logging_anthropic_collected_chunks
- Test existing behavior when request_body.model is present
- Test fallback logic when request_body.model is empty but logging_obj.model_call_details has model
- Test edge cases where both sources are empty or missing
- Ensure backwards compatibility and graceful degradation

* fix(anthropic_passthrough_logging_handler.py): add provider to model name (accurate cost tracking)

* fix(anthropic_passthrough_logging_handler.py): don't reset custom llm provider, if already set

* fix: fix check

---------

Co-authored-by: Haggai Shachar <haggai.shachar@backline.ai>

* Remove extraneous `s` in docs (#13079)

* Fix list team v2 security check (#13094)

* Fix security vulnerability in list_team_v2 endpoint

- Add missing allowed_route_check_inside_route security check to list_team_v2
- Add @management_endpoint_wrapper decorator for consistency with list_team
- Add comprehensive tests to verify security checks work correctly
- Ensure non-admin users can only query their own teams
- Ensure admin users can query all teams

This fixes a security bug where non-admin users could potentially access
team information they shouldn't have access to through the list_team_v2
endpoint, which was missing the authorization check present in list_team.

* Fix test

* Test fixes

* Fixed test

* Restored invalid delete

* Revert

---------

Co-authored-by: openhands <openhands@all-hands.dev>

* [MCP gateway] add pre and during call hooks init (#13067)

* add hook init

* add during hook

* added logging

* fix: improve MCP server URL validation to support internal/Kubernetes URLs (#13099)

* fix: improve MCP server URL validation to support internal/Kubernetes URLs

- Replace strict Ant Design URL validator with flexible custom validator
- Allow URLs like http://service-name.domain.svc.cluster.:1234/mcp
- Update both create and edit MCP server forms for consistency

* refactor: extract MCP server validation into reusable utilities

- Move URL validation logic to utils.tsx to follow DRY principles
- Add validateMCPServerUrl function for flexible URL validation
- Add validateMCPServerName function for hyphen validation
- Update both create and edit components to use shared utilities
- Reduces code duplication and improves maintainability

* [Bug Fix] Gemini-CLI - The Gemini Custom API request has an incorrect authorization format (#13098)

* fix GoogleGenAIConfig

* fix validate_environment

* test_agenerate_content_x_goog_api_key_header

* set default value for mcp namespace tool name to prevent duplicate entry in table (#12894)

* [Feat] Allow using query_params for setting API Key for generateContent routes (#13100)

* fix is_generate_content_route

* fix route checks

* fix get_api_key

* add openrouter grok4 (#13018)

* docs AZURE_CERTIFICATE_PASSWORD

* fix mcp dep for litellm (#13102)

* fix: always use choice index=0 for Anthropic streaming responses (#12666)

- Fixed 'missing finish_reason for choice 1' error with reasoning_effort
- Anthropic sends multiple content blocks with different indices
- OpenAI expects all content in a single choice at index=0
- Added comprehensive tests for text-only, text+tool, and multiple tools

* BUGFIX: Jitter should be added not multiplied (#12877) (#12901)

* Jitter should be added not multiplied

This fixes a bug mentioned in https://github.com/BerriAI/litellm/issues/12877

`JITTER=0.75` is multiplied by `random.random()` so `sleep_seconds*jitter` is a tiny number that is always less than `min_timeout`.

jitter should be added not multiplied

* Add jitter to min_timeout case also

* Cleanup jitter logic

* Always apply jitter

* fix: best practices suggest this to set to true (#12809)

The order of the specification is important here, k8s will take the last value as truth. Push down to be sure schema update is done by migration job

* fix: Set user from token user_id for OpenMeter integration (#13029)

* Revert "fix: Set user from token user_id for OpenMeter integration (#13029)" (#13107)

This reverts commit f8c09e44f6c8a2e8b8f05b193d98cc7f3cdc09c8.

* Fix fallback delete (#12606)

* fix fallbacks deletion

* x

* Fix/gemini api key environment variable support (#12507)

* Fix: Add support for GOOGLE_API_KEY environment variables for Gemini API authentication

* added test cases

* incoperated feedback to make it more maintainable

* fix failed linting CI

* [MCP Gateway] Add protocol headers (#13062)

* Add protocol headers

* fix mypy

* fix tests

* fix tests

* Fix token counter to ignore unsupported keys like prefix (#11791) (#11954)

* Custom Auth - bubble up custom exceptions  (#13093)

* fix(enterprise/litellm_enterprise/proxy/auth/user_api_key_auth.py): bubble up exception if type is ProxyException

* docs(custom_auth.md): doc on bubbling up custom exceptions

* docs(index.md): add rc docker tag

* docs(index.md): cleanup

* feat: Add dot notation support for all JWT fields (#13013)

* feat: Add dot notation support for all JWT fields

- Updated all JWT field access methods to use get_nested_value for dot notation support
- Enhanced get_team_id to properly handle team_id_default fallback with nested fields
- Added comprehensive unit tests for nested JWT field access and edge cases
- Updated documentation to reflect dot notation support across all JWT fields
- Maintains full backward compatibility with existing flat field configurations

Supported fields with dot notation:
- team_id_jwt_field, team_ids_jwt_field, user_id_jwt_field
- user_email_jwt_field, org_id_jwt_field, object_id_jwt_field
- end_user_id_jwt_field (roles_jwt_field was already supported)

Example: user_id_jwt_field: 'user.sub' accesses token['user']['sub']

* fix: Add type annotations to resolve mypy errors

- Add explicit type annotation for team_ids variable in get_team_ids_from_jwt
- Add type ignore comment for sentinel object return in get_team_id
- Resolves mypy errors while maintaining functionality

* fix: Resolve mypy type error in get_team_ids_from_jwt

- Remove explicit List[str] type annotation that conflicts with get_nested_value return type
- Simplify return logic to use 'team_ids or []' ensuring always returns List[str]
- Fixes: Incompatible types in assignment (expression has type 'list[str] | None', variable has type 'list[str]')

* fix: Add proper type annotation for team_ids variable

- Use Optional[List[str]] type annotation to satisfy mypy requirements
- Resolves: Need type annotation for 'team_ids' [var-annotated]
- Maintains functionality while ensuring type safety

* refactor: remove outdated JWT unit tests and consolidate JWT-related functionality

- Deleted the test_jwt.py file as it contained outdated and redundant tests.
- Consolidated JWT-related tests into test_handle_jwt.py for better organization and maintainability.
- Updated tests to ensure proper functionality of JWT handling, including token validation and role mapping.
- Enhanced test coverage for JWT field access and nested claims handling.

* test: add comprehensive unit tests for JWT authentication

- Introduced a new test file `test_jwt.py` containing unit tests for JWT authentication.
- Implemented tests for loading configuration with custom role names, validating tokens, and handling team tokens.
- Enhanced coverage for JWT field access, nested claims, and role-based access control.
- Added fixtures for Prisma client and public JWT key generation to support testing.
- Ensured proper handling of valid and invalid tokens, including user and team scenarios.

* revert test_handle_jwt.py

* rename file

* test: remove outdated JWT nesting tests and add new nested field access tests

- Deleted the `test_jwt_nesting.py` file as it contained outdated tests.
- Introduced new tests in `test_handle_jwt.py` to verify nested JWT field access.
- Enhanced coverage for accessing nested values using dot notation and ensured backward compatibility with flat field names.
- Added tests for handling missing nested paths and appropriate default values.
- Improved handling of metadata prefixes in nested field access.

* restore file

* [Feat] MLFlow Logging - Allow adding tags for ML Flow logging requests  (#13108)

* add mlflow tags

* fixes config

* add litellm mlflow

* test_mlflow_request_tags_functionality

* docs ML flow litellm proxy

* docs ml flow

* docs mlflow

* [LLM translation] Add support for bedrock computer use (#12948)

* Add support for bedrock computer use

* remove print

* split bedrock tools

* add hosted tools

* fix tool use

* fix tool use

* fix function calling

* fix converse transformation

* fix tests

* bump: version 1.74.10 → 1.74.11

* transform_image_generation_response

* fix transform_image_generation_response

* Revert "[MCP Gateway] Add protocol headers (#13062)"

This reverts commit 8de24bab7c3ba14c94ad34d270315f1050e693d8.

* fix test_mlflow_request_tags_functionality

* After selecting date range show loader on usage cost charts (#13113)

* prettier

* added loader in bar chart

* prettier

* added existing loader style

* make datepicker responsive

* test_user_api_key_auth

* Revert "Revert "[MCP Gateway] Add protocol headers (#13062)""

This reverts commit acd915f2dbea03a5c44c7502096a1edc4acd0a3b.

* use _safe_get_request_query_params

* test: update test

* Revert "[LLM translation] Add support for bedrock computer use (#12948)" (#13118)

This reverts commit 760d747465d9d6a07d711c04d83b136a6e285dd6.

* test: update test

* fix(model_checks.py): handle custom values in wildcard model name (e.g. genai/test/*) (#13116)

Fixes https://github.com/BerriAI/litellm/issues/13078

* move to use_prisma_migrate by default + resolve team-only models on auth checks + UI - add sagemaker on UI (#13117)

* fix(proxy_cli.py): make use_prisma_migrate proxy default

Fixes https://github.com/BerriAI/litellm/issues/13046

 Prisma migrate deploy prevents resetting db

* fix(auth_checks.py): resolve team only models while doing auth checks on model access groups

Fixes issue where key had access via an access group, but team only model could not be called

* test(test_router.py): add unit testing

* feat(provider_specific_fields.tsx): add aws sagemaker on UI

* test: update test

* fix tool aws bedrock call index when the function only have optional arg (#13115)

* docs: cleanup

* [MCP Gateway] add health check endpoints for MCP (#13106)

* add health check endpoints for MCP

* add import

* Clean up endpopints

* fix ruff

* [MCP Protocol header] fix issue with clients protocol header (#13112)

* fix headers

* fix test

* fix ruff

* fix mypy

* Added Voyage, Jinai, Deepinfra and VolcEngine providers on the UI (#13131)

* added voyage and jinai and volcengine

* deepinfra added and alphabetically ordered

* docs: cleanup

* fix object permission for orgs (#13142)

* New Advanced Date Range Picker Component (#13141)

* new date-range picker added

* remove unused utils

* [Feat] UI + Backend add a tab for use agent activity  (#13146)

* Add user agent analytics endpoints and UI for tracking client metrics

Co-authored-by: ishaan <ishaan@berri.ai>

* fix user agent analytics

* fix getting DAU

* fixes for user agent

* showing top user agents

* on this page remove Success Rate by User Agent

* fix linting

* add agent activity

* cleanup interface

* fix ruff

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: ishaan <ishaan@berri.ai>

* [LLM translation] Fix bedrock computer use (#13143)

* Add support for bedrock computer use

* remove print

* split bedrock tools

* add hosted tools

* fix tool use

* fix tool use

* fix function calling

* fix converse transformation

* fix tests

* fix llm translation test

* fix computer use

* [MCP Guardrails] move pre and during hooks to ProxyLoggin (#13109)

* move pre and during hooks t o ProxyLoggin

* fix lint

* fix ruff

* fix tests

* [Feat] v2 updates - tracking DAU, WAU, MAU for coding tool usage + show Daily Usage per User (#13147)

* Add user agent analytics endpoints and UI for tracking client metrics

Co-authored-by: ishaan <ishaan@berri.ai>

* fix user agent analytics

* fix getting DAU

* fixes for user agent

* showing top user agents

* on this page remove Success Rate by User Agent

* fix linting

* add agent activity

* cleanup interface

* fix ruff

* round cost

* fix charts

* fixes - show DAU, MAU, WAU

* move to a diff file

* fix

* fixes for user agent analytics

* fix user_agent_analytics_endpoints

* fix mypy linting

* fix linting

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: ishaan <ishaan@berri.ai>

* Litellm explore postgres db ci cd (#13156)

* ci(config.yml): testing with ci/cd db

* build: spin up pg db in ci/cd test

* [MCP Gateway] Litellm mcp client list fail (#13114)

* fix headers

* fix test

* fix ruff

* added try except for catching errors which lead to client failures

* fix mypy

* fix ruff

* fix tests

* fix python error

* fix test

* fix test

* fixed the MCP Call Tool result

* ci: remove bad script

* ci(config.yml): run prisma generate before running enterprise tests

* fix grype scan

* build(pyproject.toml): bump version

* ci: migrate to db in pipeline

* fix migrations (#13157)

* Revert "[LLM translation] Fix bedrock computer use (#13143)"

This reverts commit 840dd2e7c7812a2967890593e24de06c1f658adb.

* poetry lock

* test: handle api instability

* ci(config.yml): remove check

* ci: migrate to postgres in ci/cd

* test fix xai - it goes through base llm tests already

* build(config.yml): migrate build_and_test to ci/cd pg db (#13166)

* add framework name to UserAgent header in AWS Bedrock API call (#13159)

* fix: remove obsolete attribute `version` in docker compose (#13172)

Fix the warning: WARN[0000] docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion

* test_chat_completion_ratelimit

* Revert "add framework name to UserAgent header in AWS Bedrock API call (#13159)"

This reverts commit 77f506e860654252797f0f555c55f166cd04762c.

* [Feat] Background Health Checks - Allow disabling background health checks for a specific  (#13186)

* disable background health checks for specific models

* test_background_health_check_skip_disabled_models

* Disable Background Health Checks For Specific Models

* [Proxy Startup]fix db config through envs (#13111)

* fix db config through envs

* add helper

* fix ruff

* fix imports

* add unit tests in db config changes

* UI - new build

* fix: support negative indexes in cache_control_injection_points for Anthropic Claude (#10226) (#13187)

* [Bug Fix] Gemini-CLI Integration - ensure tool calling works as expected on generateContent (#13189)

* transform_generate_content_request

* add tools in GenerateContentRequestDict

* add generate_content_handler tool calling

* google_generate_content_endpoint_testing

* test_mock_stream_generate_content_with_tools

* test_validate_post_request_parameters

* fixes for generate_content_handler

* fix VertexAIGoogleGenAIConfig

* fixes veretx ai

* google_generate_content_endpoint_testing

* test_async_streaming_with_logging

* load_vertex_ai_credentials

* test_vertex_anthropic.py

* [Bug Fix] Infra - ensure that stale Prisma clients disconnect DB connection  (#13140)

* ensure original client is disconnected when re-creating

* test_recreate_prisma_client_successful_disconnect

* test_recreate_prisma_client_successful_disconnect

* [Feat] Allow redacting message / response content for specific logging integrations - DD LLM Observability (#13158)

* fix redact_standard_logging_payload

* add StandardCustomLoggerInitParams

* allow defining DatadogLLMObsInitParams

* fix init DataDogLLMObsLogger

* fix import

* update redact_standard_logging_payload_from_model_call_details

* test_dd_llms_obs_redaction

* docs DD logging

* docs DD

* docs DD

* Redacting Messages, Response docs DD LLM Obs

* fix redaction logic

* fix create_llm_obs_payload

* fix logging response

* fixes

* ruff fix

* fix test

* test_dd_llms_obs_redaction

* test_create_llm_obs_payload

* redact_standard_logging_payload_from_model_call_details

* img - dd_llm_obs

* docs DD

* fix linting

* fix linting

* fix mypy

* test_create_llm_obs_payload

* test_create_llm_obs_payload

* fix mock_env_vars

* fix _handle_anthropic_messages_response_logging

* Litellm fix fallbacks UI (#13191)

* UI - fix setting fallbacks on UI

* fix add fallbacks

* ui polish

* fix: correct patch path in langfuse test for MAX_LANGFUSE_INITIALIZED_CLIENTS (#13192)

The test was failing because it was trying to patch MAX_LANGFUSE_INITIALIZED_CLIENTS
at the wrong path. The constant is imported from litellm.constants into the langfuse
module namespace, so we need to use patch.object on the imported module reference.

Changes:
- Import langfuse module explicitly for patching
- Use patch.object instead of patch string path
- This fixes the AttributeError that was causing CI failures

* ui new build

* add When to Use Each Endpoint (#13193)

* Fix - using managed files w/ OTEL + UI - add model group alias on UI (#13171)

* fix(router.py): safe deep copy kwargs

OTEL adds a parent_otel_span which cannot be deepcopied

* fix: use safe deep copy in other places as well

* test: add script to check and ban copy.deepcopy of kwargs

enforce safe_deep_copy usage

* build(ui/): new component for adding model group alias on UI

* fix(proxy_server.py): support updating model_group_alias via /config/update

allows ui component to work

* fix(router.py): update model_group_alias in router settings based on db value

* fix: fix code qa error

* Anthropic - working mid-stream fallbacks  (#13149)

* fix(router.py): add acompletion_streaming_iterator inside router

allows router to catch errors mid-stream for fallbacks

Work for https://github.com/BerriAI/litellm/issues/6532

* fix(router.py): working mid-stream fallbacks

* fix(router.py): more iterations

* fix(router.py): working mid-stream fallbacks with fallbacks set on router

* fix(router.py): pass prior content back in new request as assistant prefix message

* fix(router.py): add a system prompt to help guide non-prefix supporting models to use the continued text correctly

* fix(common_utils.py): support converting `prefix: true` for non-prefix supporting models

* fix: reduce LOC in function

* test(test_router.py): add unit tests for new function

* test: add basic unit test

* fix(router.py): ensure return type of fallback stream is compatible with CustomStreamWrapper

prevent client code from breaking

* fix: cleanup

* test: update test

* fix: fix linting error

* Anthropic - mid stream fallbacks p2 (add token usage across both calls) (#13170)

* fix(router.py): add acompletion_streaming_iterator inside router

allows router to catch errors mid-stream for fallbacks

Work for https://github.com/BerriAI/litellm/issues/6532

* fix(router.py): working mid-stream fallbacks

* fix(router.py): more iterations

* fix(router.py): working mid-stream fallbacks with fallbacks set on router

* fix(router.py): pass prior content back in new request as assistant prefix message

* fix(router.py): add a system prompt to help guide non-prefix supporting models to use the continued text correctly

* fix(common_utils.py): support converting `prefix: true` for non-prefix supporting models

* fix: reduce LOC in function

* test(test_router.py): add unit tests for new function

* test: add basic unit test

* fix(router.py): ensure return type of fallback stream is compatible with CustomStreamWrapper

prevent client code from breaking

* fix: cleanup

* test: update test

* fix: fix linting error

* fix(router.py): return combined usage

ensures accurate usage tracking on clientside for stream w/ fallbacks

* [UI QA] QA - Agent Activity Tab  (#13203)

* backend fixes

* fixes for User-Agent ui

* UI fixes chart loader

* fixes chart loader

* fixes ChartLoader

* fix ChartLoader

* fixes for analytics

* Fix/panw prisma airs post call hook (#13185)

* fix(guardrails): Fix PANW Prisma AIRS post-call hook method name

- Changed async_post_call_hook to async_post_call_success_hook to match proxy calling convention
- Added event_hook parameter to initialization to ensure proper hook registration
- Fixes post-call response scanning for PANW Prisma AIRS guardrails

Resolves issue where post-call hooks were not being invoked due to method name mismatch.

* Update PANW Prisma AIRS tests to use correct method name

* allow helm hooks for migrations job (#13174)

* add openssl in apk install in runtime stage in dockerfile.non_root (#13168)

* add openssl in apk install in runtime stage in dockerfile.non_rootdocker-compose logs -f litellm

* Improve Docker-compose.yaml for local debugging

---------

Co-authored-by: Krish Dholakia <krrishdholakia@gmail.com>

* bump: version 1.74.12 → 1.74.13

* bump: version 1.74.13 → 1.74.14

* Prompt Management - add local dotprompt file support

* allow redifine base api url in pass trough (#13134)

* Fix API Key Being Logged (#12978)

* AIF-262 Fix for keys being logged

* AIF-262 Undid http exception detail removal

* AIF-262 Converted f-string to normal

* build(config.yml): use ci/cd postgres in test

* fix(litellm_logging.py): fix merge error

* test: update test

* test: update tests

* test: update tests

* test: loosen check

* build(ui/): fix linting errors

* fix(model_group_alias_settings.tsx): fix check

* test: remove bad unit tests

* test: update unit tests

* docs(index.md): cleanup

* Index.md - cleanup docs (#13215)

* docs: add highlights

* docs(index.md): add model-level guardrails

* docs(index.md): cleanup notes

* docs: fix docs

* docs: add more details

* docs(index.md): cleanup doc

* [LLM translation] Fix bedrock computer use #13143 (#13150)

* fix json test

* fix pr

* fix bedrock computer use tool

* added unit test

* fix failing prisma tesT

* fix prisma connect

* docs(index.md): cleanup

* [QA] Viewing Agent Activity Headers on UI Usage Page (#13212)

* qa - agents

* refactored WAU, MAU and DAU endpoints

* fixes for dau, wau, mau

* use stack=true

* fixes for DAU calc

* fixes for rendering WAU, MAU

* use 1 section for topline

* Fixes for endpoint

* remove filter

* fix spacing

* fix activity

* working UI rendering

* fixes for chart data

* allow selecting specific tags

* add DistinctTagResponse endpoints

* use wide selector

* add types

* fixes for UI rendering

* get_per_user_analytics

* test_recreate_prisma_client_successful_disconnect

* ui new build

* fix vertex deprecated old model

* [Separate Health App] Update Helm Deployment.yaml (#13162)

* add helm deployment fix

* clean deployment

* [Proxy]fix key mgmt (#13148)

* fix key mgmt

* Add unit test

* [LLM] fix model reload on model update (#13216)

* fix model reload on model update

* remove the flag

* suppress httpx logging (#13217)

* [MCP Gateway] Litellm mcp pre and during guardrails (#13188)

* add guardrail support

* add guardrail support

* guardrails for MCP

* added changes

* add mcp guardrails

* added test

* add ui

* fix guardrail form

* working with cursor

* remvoe print

* fix mcp servertests

* fix mypy and remove console logs

* fix mypy and remove console logs

* fix mypy tests

* testing fixes - vertex ai deprecated claude 3 sonnet models

* Add advanced date picker to all the tabs on the usage page (#13221)

* advancedatepicker for tag usage and team usage

* reduce white space in date picker

* selected time range option is visible

* dont wait for apply button to select relative time options

* add Perplexity citation annotations support (#13225)

* fix: role chaining and session name with webauthentication for aws bedrock (#13205)

* fix(bedrock): prevent duplicate role assumption in EKS/IRSA environments

Fixes issue where AWS role assumption would fail in EKS/IRSA environments
when trying to assume the same role that's already being used.

The problem occurred when:
1. EKS/IRSA automatically assumes a role (e.g., LitellmRole)
2. LiteLLM tries to assume the same role again, causing AccessDenied errors
3. Different models with different roles would fail due to incorrect role context

Changes:
- Added check in _auth_with_aws_role() to detect if already using target role
- Skip role assumption if current identity matches target role
- Return current credentials instead of attempting duplicate assumption
- Added comprehensive test coverage for the fix

This ensures proper role chaining works in EKS/IRSA environments where:
- Service Account can assume Role A
- Role A can assume Role B for different models/accounts

Resolves the AccessDenied errors reported in bedrock usage scenarios.

* fix(bedrock): simplify role assumption for EKS/IRSA environments

Fixes AWS Bedrock role assumption in EKS/IRSA environments by properly
handling ambient credentials when no explicit credentials are provided.

The issue occurred because commit 197e7efa8f097bb935cf86dc4100422487a40955
introduced changes that broke role assumption in EKS/IRSA environments.

Changes:
- Simplified _auth_with_aws_role() to use ambient credentials when no
  explicit AWS credentials are provided (aws_access_key_id and
  aws_secret_access_key are both None)
- This allows web identity tokens in EKS/IRSA to work automatically
  through boto3's credential chain
- Maintains backward compatibility for explicit credential scenarios

Added comprehensive test coverage:
- test_eks_irsa_ambient_credentials_used: Verifies ambient credentials work
- test_explicit_credentials_used_when_provided: Ensures explicit creds still work
- test_partial_credentials_still_use_ambient: Edge case handling
- test_cross_account_role_assumption: Multi-account scenarios
- test_role_assumption_with_custom_session_name: Custom session names
- test_role_assumption_ttl_calculation: TTL calculation verification
- test_role_assumption_error_handling: Error propagation
- test_multiple_role_assumptions_in_sequence: Sequential role assumptions

This fix ensures that in EKS/IRSA environments:
1. Service accounts can assume their initial role via web identity
2. That role can then assume other roles across accounts as configured
3. Different models can use different roles without conflicts

* fix(bedrock): add automatic IRSA detection for EKS environments

- Detect AWS_WEB_IDENTITY_TOKEN_FILE and AWS_ROLE_ARN environment variables
- Automatically use web identity token flow when IRSA is detected
- Read web identity token from file and pass to existing auth method
- Add test coverage for IRSA environment detection
- Fixes authentication errors in EKS with IRSA when no explicit credentials provided

* fix(bedrock): skip role assumption when IRSA role matches requested role

- Detect when AWS_ROLE_ARN environment variable matches the requested role
- Skip unnecessary role assumption when already running as the target role
- Use existing env vars authentication method for IRSA credentials
- Add test coverage for same-role IRSA scenario
- Fixes 'not authorized to perform: sts:AssumeRole' errors when trying to assume the same role

* fix(bedrock): use boto3's native IRSA support for cross-account role assumption

- Replace custom web identity token handling with boto3's built-in IRSA support
- boto3 automatically reads AWS_WEB_IDENTITY_TOKEN_FILE and assumes initial role
- Then use standard assume_role for cross-account access
- Update test to mock boto3 STS client instead of internal methods
- Fixes 'OIDC token could not be retrieved from secret manager' error

* fix(bedrock): improve IRSA error handling and add debug logging

- Add debug logging to show current identity and role assumption attempts
- Provide clearer error messages for trust policy issues
- Fix region handling in IRSA flow
- Re-raise exceptions instead of silently falling through
- This helps diagnose cross-account role assumption permission issues

* fix(bedrock): manually assume IRSA role with correct session name for cross-account scenarios

- When doing cross-account role assumption, manually assume the IRSA role first with the desired session name
- This ensures the session name in the assumed role ARN matches what's expected in trust policies
- For same-account scenarios, continue using boto3's automatic IRSA support
- Updated tests to handle the new flow
- This fixes the issue where cross-account trust policies require specific session names

* fix: Fix linting issues in base_aws_llm.py

- Fix f-string without placeholders (F541)
- Refactor _auth_with_aws_role to reduce statements count (PLR0915)
  - Extract _handle_irsa_cross_account helper method
  - Extract _handle_irsa_same_account helper method
  - Extract _extract_credentials_and_ttl helper method

---------

Co-authored-by: openhands <openhands@all-hands.dev>

* Fix missing extra_headers support for vLLM/openai_like embeddings (#13198)

- Add extra_headers handling to hosted_vllm/openai_like embedding providers
- Matches existing pattern used in OpenAI embeddings section
- Fixes issue where custom headers were dropped for vLLM embedding requests

Fixes #13088

* litellm/proxy: preserve model order of /v1/models and /model_group/info (#13178)

Closes #12644

Signed-off-by: Alexander Yastrebov <alexander.yastrebov@zalando.de>

* Prompt Management - abstract prompt templates away from model list (enables permission management on prompt templates)  (#13219)

* feat: initial commit with prompt management support on pre-call hooks

allows prompt templates to work before assigning specific models

* feat: initial logic for independent prompt management settings

* feat(proxy_server.py): working logic for loading in the prompt templates from config yaml

allows creating an independent 'prompts' section in the config yaml

* feat(prompt_registry.py): working e2e custom prompt templates with guardrails and models

* refactor(prompts/): move folder inside proxy folder

easier management for prompt endpoints

* fix: fix linting error

* fix: fix check

* [QA Fixes for MCP] - Ensure MCPs load + don't run a health check everytime we load MCPs on UI (#13228)

* qa - mcps should load even if they don't have required fields

* fix loading MCPs

* Revert "fix: role chaining and session name with webauthentication for aws be…" (#13230)

This reverts commit 0ac093b59edab48b1400bc84e133a35ef4accfa2.

* fix(proxy_setting_endpoints.py): don't block startup if team doesn't exist in default team member budget

* Prompt Management (2/2) - New `/prompt/list` endpoint + key-based access to prompt templates (#13218)

* feat: initial commit with prompt management support on pre-call hooks

allows prompt templates to work before assigning specific models

* feat: initial logic for independent prompt management settings

* feat(proxy_server.py): working logic for loading in the prompt templates from config yaml

allows creating an independent 'prompts' section in the config yaml

* feat(prompt_registry.py): working e2e custom prompt templates with guardrails and models

* refactor(prompts/): move folder inside proxy folder

easier management for prompt endpoints

* feat(prompt_endpoints.py): working `/prompt/list` endpoint

returns all available prompts on proxy

* feat(key_management_endpoints.py): support storing 'prompts' in key metadata

allows giving keys access to specific prompts

* feat(prompt_endpoints.py): enable key-based access to /prompts/list

ensures key can only see prompts it has access to

* fix(init_prompts.py): fix linting error

* fix: fix ruff check

* fix(proxy/_types.py): add 'prompts' to newteamrequest

* fix(litellm_logging.py): update logged message with scrubbed value

* truncateUserAgent

* [UI QA Fixes] Stable release (#13231)

* qa - user agent view

* fixes for usage time selector

* Revert "Fix SSO Logout | Create Unified Login Page with SSO and Username/Password Options (#12703)"

This reverts commit a752d7acc9f9db145d0b1d49ddb53263b67d0b31.

* Revert "Revert "Fix SSO Logout | Create Unified Login Page with SSO and Username/Password Options (#12703)""

This reverts commit 5fe37b6f72060add859a22ddda0665cd1635f98f.

* fixes - ui login with SSO

* doc fix - missing "prompts" in /key endpoint swagger

* ui new build

* bump: version 1.74.14 → 1.74.15

* ruff fix

* docs release notes

* fixes MCP gateway docs

* [docs release notes] (#13237)

* docs release notes

* docs release notes

* docs rnotes

* docs api version

* fixes docs

* docs rn

* docs computer use

* docs RC

* docs - Track Usage for Coding Tools

* docs cost tracking coding

* agent 4.png

* docs fix

* docs fix

* docs fix

* docs User Agent Activity Tracking

* UI - Add giving keys prompt access (#13233)

* fix(create_key_button.tsx): add prompts on UI

* feat(key_management_endpoints.py): support adding prompt to key via `/key/update`

* fix(key_info_view.tsx): show existing prompts on key in key_info_view.tsx

* fix(key_edit_view.tsx): UX - disable premium feature for non-premium users

prevent accidental clicking

* fix(create_key_button.tsx): disable premium features behind flag, prevent errors

* fix(key_management_endpoints.py): fix key update logic \

* fix: fix check

* docs: document new params

* Prompt Management - Add table + prompt info page to UI  (#13232)

* fix(create_key_button.tsx): add prompts on UI

* feat(key_management_endpoints.py): support adding prompt to key via `/key/update`

* fix(key_info_view.tsx): show existing prompts on key in key_info_view.tsx

* fix(key_edit_view.tsx): UX - disable premium feature for non-premium users

prevent accidental clicking

* fix(create_key_button.tsx): disable premium features behind flag, prevent errors

* feat(prompts.tsx): add new ui component to view created prompts

enables viewing prompts created on config

* feat(prompt_info.tsx): add component for viewing the prompt information

* Prompt Management - add prompts on UI  (#13240)

* fix(create_key_button.tsx): add prompts on UI

* feat(key_management_endpoints.py): support adding prompt to key via `/key/update`

* fix(key_info_view.tsx): show existing prompts on key in key_info_view.tsx

* fix(key_edit_view.tsx): UX - disable premium feature for non-premium users

prevent accidental clicking

* fix(create_key_button.tsx): disable premium features behind flag, prevent errors

* feat(prompts.tsx): add new ui component to view created prompts

enables viewing prompts created on config

* feat(prompt_info.tsx): add component for viewing the prompt information

* feat(prompt_endpoints.py): support converting dotprompt to json structure + accept json structure in promptmanager

allows prompt manager to work with api endpoints

* test(test_prompt_manager.py): add unit tests for json data input

* feat(dotprompt/__init__.py): add prompt data to dotpromptmanager

* fix(prompt_endpoints.py): working crud endpoints for prompt management

* feat(prompts/): support `prompt_file` for dotprompt

allows to precisely point to the prompt file a prompt should use

* feat(proxy/utils.py): resolve prompt id correctly

resolves user sent prompt id with internal prompt id

* feat(schema.prisma): initial pr with db schema for prompt management table

allows post endpoints to work with backend

* feat(prompt_endpoints.py): use db in patch_prompt endpoint

* feat(prompt_endpoints.py): use db for update_prompt endpoint

* feat(prompt_endpoints.py): use db on prompt delete endpoint

* build(schema.prisma): add prompt tale to schema.prisma in litellm-proxy-extras

* build(migration.sql): add new sql migration file

* fix(init_prompts.py): fix init

* feat(prompt_info_view.tsx): show the raw prompt template on ui

allows developer to know the prompt template they'll be calling

* feat(add_prompt_form.tsx): working ui add prompt flow

allows user to add prompts to litellm via ui

* build(ui/): styling fixes

* build(ui/): prompts.tsx

styling improvements

* fix(add_prompt_form.tsx): styling improvements

* build(prompts.tsx): styling improvements

* build(ui/): styling improvements

* build(ui/): fix ui error

* fix: fix ruff check

* docs: document new api params

* test: update tests

* fix openshift (#13239)

* build: update poetry

* fix(key_management_endpoints.py): fix check

* docs(index.md): cleanup

* [LLM Translation] Fix Model Usage not having text tokens (#13234)

* fix + test

* remove test comments

* fix mypy

* fix mypy

* fix tests

* [UI] Add team deletion check for teams with keys (#12953)

* added check option

* Add underline

* make less verbosE

* [Bug Fix] OpenAI / Azure Responses API - Add `service_tier` , `safety_identifier` supported params (#13258)

* test_aresponses_service_tier_and_safety_identifier

* add service_tier + safety_identifier

* fix get_supported_openai_params

* add safety_identifier + service_tier for responses()

* Bug Fix - Responses API raises error with Gemini Tool Calls in `input` (#13260)

* add _transform_responses_api_function_call_to_chat_completion_message

* test_responses_api_with_tool_calls

* TestFunctionCallTransformation

* fixes for responses API testing google ai studio

* TestGoogleAIStudioResponsesAPITest

* test_responses_api_with_tool_calls

* test_responses_api_with_tool_calls

* test_basic_openai_responses_streaming_delete_endpoint

* docs(index.md): cleanup tag

* docs(user_keys.md): add litellm python sdk tab

* Update model_prices_and_context_window.json (#13244)

* [Bug Fix] Fix  Server root path regression on UI when using "Login" (#13267)

* bug fix serve_login_page

* test_serve_login_page_server_root_path

* Support OCI provider (#13206)

* create OCI required files

* request and response conversion for non-streaming chat

* support tool calling with OCI generic API without streaming

* adaptation of api call for generic and cohere format

* include tool calls and responses in generic api and dropping support for cohere

* fix invalid content-length error

* support streaming for generic api

* fix auth error when using acompletion with streaming

* refactor: use base_llm_http_handler and include API type definitions

* update types and add type safety in different methods

* fix OCIFunction format

* create custom stream wrapper for decoding OCI stream

* remove unused files

* create unit tests for OCI

* lint the code

* remove manual test

* docs: update the docs to include OCI

* Add GCS bucket caching support (#13122)

* Fix: Langfuse reporting "client closed" error due to httpx client TTL (#13045)

* Fix: Langfuse reporting "client closed" error due to httpx client TTL

* remove log

* add correct pricing (#13269)

* refactor(oci/chat/transformation.py): lazy load package imports

* [Bug Fix] Prometheus - fix for `litellm_input_tokens_metric`, `litellm_output_tokens_metric`  - Note this updates the metric name  (#13271)

* fixes for litellm_tokens_metric

* test_prometheus_token_metrics_with_prometheus_config

* bump: version 1.74.15 → 1.75.0

* bump: version 1.75.0 → 1.75.1

* add litellm-enterprise==0.1.17

* input cost per token higher than 1 test (#13270)

* [LLM Translation] Support /v1/models/{model_id} retrieval (#13268)

* added model id endpoint

* fix test

* add route to internal users

* make the functions reusable

* fixed mypy

* [UI] - Add ability to set model alias per key/team (#13276)

* update model alias on keys

* team model aliases

* fix model aliases

* fixes for teams

* fix OCI linting errors  (#13279)

* fix(types/llms/oci.py): fix linting errors

* fix(oci.py): fix linting error

* fix(oci.py): fix linting errors

* fix: fix linting error

* fix: fix linting error

* Ensure disable_llm_api_endpoints works + Add wildcard model support for 'team-byok' model  (#13278)

* fix(route_checks.py): ensure disable llm api endpoints is correctly set

* fix(route_checks.py): raise httpexception

raise expected exceptions

* fix(router.py): handle team only wildcard models

fixes issue where team only wildcard models were not considered during auth checks

* fix(router.py): handle team only wildcard models

fixes issue where team only wildcard models were not considered during auth checks

* fix(main.py): handle tool being a pydantic object (#13274)

* fix(main.py): handle tool being a pydantic object

Fixes https://github.com/BerriAI/litellm/issues/13064

* fix(prompt_templates/common_utils.py): fix unpack defs deepcopy issue

Fixes https://github.com/BerriAI/litellm/issues/13151

* fix(utils.py): handle tools is none

* support `thinking` field in payload

---------

Signed-off-by: Emmanuel Ferdman <emmanuelferdman@gmail.com>
Signed-off-by: Alexander Yastrebov <alexander.yastrebov@zalando.de>
Co-authored-by: Ishaan Jaff <ishaanjaffer0324@gmail.com>
Co-authored-by: Santosh Dhaladhuli <80815111+SantoshDhaladhuli@users.noreply.github.com>
Co-authored-by: Murad Khafizov <101127600+murad-khafizov@users.noreply.github.com>
Co-authored-by: Gaston <grodriguez160597@gmail.com>
Co-authored-by: Cole McIntosh <82463175+colesmcintosh@users.noreply.github.com>
Co-authored-by: sings-to-bees-on-wednesdays <222684290+sings-to-bees-on-wednesdays@users.noreply.github.com>
Co-authored-by: Dmitriy Alergant <93501479+DmitriyAlergant@users.noreply.github.com>
Co-authored-by: Krish Dholakia <krrishdholakia@gmail.com>
Co-authored-by: Jugal D. Bhatt <55304795+jugaldb@users.noreply.github.com>
Co-authored-by: Emerson Gomes <emerson.gomes@thalesgroup.com>
Co-authored-by: Matthias Dittrich <matthi.d@gmail.com>
Co-authored-by: Christoph Koehler <christoph@zerodeviation.net>
Co-authored-by: Emmanuel Ferdman <emmanuelferdman@gmail.com>
Co-authored-by: stellasec <stella@stellasec.com>
Co-authored-by: direcision <direcision@gmail.com>
Co-authored-by: Richard Tweed <RichardoC@users.noreply.github.com>
Co-authored-by: Alex Strick van Linschoten <strickvl@users.noreply.github.com>
Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: tanjiro <56165694+NANDINI-star@users.noreply.github.com>
Co-authored-by: Felix Burmester <57833596+Ne0-1@users.noreply.github.com>
Co-authored-by: Haggai Shachar <haggai.shachar@backline.ai>
Co-authored-by: Max Rabin <927792+maxrabin@users.noreply.github.com>
Co-authored-by: Tim O'Farrell <tofarr@gmail.com>
Co-authored-by: openhands <openhands@all-hands.dev>
Co-authored-by: Shao-Kan Chu <shao.chu@mail.utoronto.ca>
Co-authored-by: Maksim <74874309+Maximgitman@users.noreply.github.com>
Co-authored-by: Pathikrit Bhowmick <pathikritbhowmick@msn.com>
Co-authored-by: Marvin Huetter <61065254+huetterma@users.noreply.github.com>
Co-authored-by: Better than breakfast. <adr.viper@gmail.com>
Co-authored-by: zengxu <zengxu_121@126.com>
Co-authored-by: Siddharth Sahu <112792547+sahusiddharth@users.noreply.github.com>
Co-authored-by: Amit Kumar <123643281+Amit-kr26@users.noreply.github.com>
Co-authored-by: Johnny.H <jnhyperion@gmail.com>
Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: ishaan <ishaan@berri.ai>
Co-authored-by: 0x-fang <fanggong@amazon.com>
Co-authored-by: Kowyo <kowyo@outlook.com>
Co-authored-by: Anand Khinvasara <kanand90@gmail.com>
Co-authored-by: Jason Roberts <51415896+jroberts2600@users.noreply.github.com>
Co-authored-by: unique-jakub <jakub@unique.ch>
Co-authored-by: Mateo Di Loreto <101841200+mdiloreto@users.noreply.github.com>
Co-authored-by: Dmitry Tyumentsev <56769451+tyumentsev4@users.noreply.github.com>
Co-authored-by: aayush-malviya-acquia <aayush.malviya@acquia.com>
Co-authored-by: Sameer Kankute <135028480+kankute-sameer@users.noreply.github.com>
Co-authored-by: Alexander Yastrebov <yastrebov.alex@gmail.com>
Co-authored-by: Benjamin Bartels <benjamin@bartels.dev>
Co-authored-by: breno-aumo <160534746+breno-aumo@users.noreply.github.com>
Co-authored-by: Pascal Bro <git@pascalbrokmeier.de>
Co-authored-by: Perling <linsmiling@sina.cn>
```

[dodoman-sun](https://github.com/dodoman-sun) referenced this pull request in dodoman-sun/litellm [ Aug 5, 2025 ](https://github.com/BerriAI/litellm/pull/12988#ref-commit-fcfa0a8)
Partially verified
#  Partially verified 
This commit was created on GitHub.com and signed with GitHub’s **verified signature**. 
We cannot verify signatures from co-authors, and some of the co-authors attributed to this commit require their commits to be signed. 

```
* ui new build

* bump: version 1.74.8 → 1.74.9

* [Feat] Add inpainting support and corresponding tests for Amazon Nova Canvas (#12949)

* Added documentation about metadata exposed over the /v1/models endpoint (#12942)

* Fix: Shorten Gemini tool_call_id for Azure compatibility (#12941)

* feat: Update model pricing and context window configurations (#12910)

- Adjusted input and output cost per token for existing models.
- Added new model configuration for "openrouter/qwen/qwen3-coder" with specified token limits and costs.

* fix(auth_utils): make header comparison case-insensitive (#12950)

If the user specified in the configuration e.g. "user_header_name:
X-OpenWebUI-User-Email", here we were looking for a dict key
"X-OpenWebUI-User-Email" when the dict actually contained
"x-openwebui-user-email".

Switch to iteration and case insensitive string comparison instead to
fix this.

This fixes customer budget enforcement when the customer ID is passed
in as a header rather than as a "user" value in the body.

* GuardrailsAI: use validatedOutput to allow usage of "fix" guards. Previously "fix" guards had no effect in llmOutput mode. (#12891)

* Show global retry policy on UI  (#12969)

* fix(router.py): return global retry policy on `get/config/callbacks`

Partial fix for https://github.com/BerriAI/litellm/issues/12855

* fix(model_dashboard.tsx): accept global retry policy

Fixes https://github.com/BerriAI/litellm/issues/12855

* fix(model_dashboard.tsx): update global retry policy, if that's what was edited

* Guardrails - support model-level guardrails  (#12968)

* fix(custom_guardrail.py): initial logic for model level guardrails

* feat(custom_guardrail.py): working pre call guardrails

* fix(custom_guardrails.py): check if custom guardrails set before running event hook

* test(test_custom_guardrail.py): add unit tests for async pre call deployment hook on custom guardrail

* feat(custom_guardrail.py): add post call processing support for guardrails

allows model based guardrails to run on the post call event for that model only

* fix(utils.py): only run if call type is in enum

* test: update unit tests to work

* docs Health Check Server

* docs update

* docs update

* fix mapped test

* docs - auto routing

* docs auto routing

* docs - auto router on litellm proxy

* docs auto router

* fix ci/cd testing

* docs fix link

* build(github/manual_pypi_publish.yml): manual workflow to publish pip package - used for pushing dev releases (#12985)

* build(github/manual_pypi_publish.yml): manual workflow to publish pip package - used for pushing dev releases

* ci: remove redundant file

* [LLM Translation] Add bytedance/ui-tars-1.5-7b on openrouter (#12882)

* add bytedance model

* add source

* clean and verify key before inserting (#12840)

* clean and verify key

* change checking logic

* Add unit test

* [LLM Translation] fix query params for realtime api intent (#12838)

* fix query params for realtime api intent

* fix my py

* Add typed dict

* remove typed dict

* fix comments

* add test

* add test

* added proxt log revert

* add real time q params

* remove features from enterprise (#12988)

* feat(proxy/utils.py): support model level guardrails on stream event

enables guardrails to work with streaming

* feat(proxy_server.py): support checking full str on streaming guardrails post call hook

ensures streaming guardrails are actually useful

* build: update pip package (#12998)

* Fix issue writing db (#13001)

* add fix for redaction (#13005)

* [MCP Gateway] add Litellm mcp alias for prefixing (#12994)

* change alias-> server_name

* add server alias uses

* add tests

* schema

* ruff fix

* fix alias for config

* fix tests

* add alias

* fix tests

* fix tests

* add a common util

* ruff fix

* fix migration

* Fixup ollama model listing (again) (#13008)

* [Vector Store] make vector store permission management OSS (#12990)

* add vector store on ui behind enterprise in vector store

* remove enterprise

* [FEAT] Model-Guardrails: Add on UI (#13006)

* feat(proxy_server.py): working guardrails on streaming output

ensures guardrail actually raises an error if flagged during streaming output

* test: add unit tests

* feat(advanced_settings.tsx): add guardrails option as ui component on model add

enables setting guardrails on model add

* feat(add_model_tab.tsx): fix add model form

* feat(model_info_view.tsx): support adding guardrails on model update

* fix(add_model_tab.tsx/): working health check when guardrails selected

* fix(proxy_server.py): fix yield

* UI SSO - fix reset env var when ui_access_mode is updated  (#13011)

* fix(ui_sso.py): fix form action on login when sso is enabled

* fix: multiple fixes - fix resetting env var in proxy config + add key to exception message on key decryption

fixes issue where env vars would be reset

* refactor(proxy_server.py): cleanup redundant decryption line

* fix(proxy_setting_endpoints.py): show saved ui access mode

allows admin to know what they'd previously stored in db

* [MCP Gateway] Litellm mcp multi header propagation (#13003)

* change alias-> server_name

* add server alias uses

* add tests

* schema

* ruff fix

* fix alias for config

* fix tests

* add alias

* fix tests

* add multi server header support

* add and fix tests

* fix tests

* fix tests

* add a common util

* ruff fix

* fix ruff

* fix tests

* fix migration

* mypy fix

* change server py

* test_router_auto_router

* Litellm release notes 07 27 2025 p1 (#13027)

* docs(index.md): initial commit for v1.74.9-stable release note

* docs(index.md): add more cost tracking models

* docs(index.md): add new llm api endpoints + mcp gateway features

* docs: add logging/guardrail improvements

* docs(index.md): complete initial draft

* build(model_prices_and_context_window.json): fix or pricing

* build(model_prices_and_context_window.json): fix or pricing

* test: fix test

* VertexAI - camelcase optional params for image generation + Anthropic - streaming, always ensure assistant role set on only first chunk (#12889)

* fix(vertex_ai/image_generation): transform `_` param to camelcase

Fixes https://github.com/BerriAI/litellm/issues/12690

* test(test_vertex_image_generation.py): add unit tests

* fix(streaming_handler.py): assert only 1 assistant chunk in stream

Fixes https://github.com/BerriAI/litellm/issues/12616

* fix(streaming_handler.py): fix check

* Bulk User Edit - additional improvements - edit all users + set 'no-default-models' on all users (#12925)

* feat(bulk_user_update/): support updating all users on proxy

* fix(bulk_edit_user.tsx): persist user settings when 'add to team' clicked

* fix(team_endpoints.py): bulk add all proxy users to team

supports flow from UI to add all existing users to a team

* fix: minor fixes

* feat(user_edit_view.tsx): support setting no default model on user edit

allows preventing users from calling models outside team scope

* fix(user_edit_view.tsx): prevent triggering submit when 'cancel' is clicked

* refactor(internal_user_endpoints.py): refactor to reduce function size

* build: build new ui

* fix(proxy_settings_endpoints.py): fix clearing SSO settings

* refactor(create_key_button.tsx): cleanup read only option (confusing)

* build: update ui build

* test: update logic to fix for unit tests

* fix: add X-Initiator header for GitHub Copilot to reduce premium requests (#13016)

- Implement X-Initiator header logic in GithubCopilotConfig.validate_environment()
- Set header to "agent" when messages contain agent or tool roles, "user" otherwise
- Reduces unnecessary premium Copilot API usage for non-user calls

Fixes #12859

* docs - openweb show how to include reasoning content (#13060)

* build: bump pip

* [Bug Fix] Pass through logging handler VertexAI - ensure multimodal embedding responses are logged  (#13050)

* fix _is_multimodal_embedding_response

* test_vertex_passthrough_handler_multimodal_embedding_response

* Remove duplicate test case verifying field filtering logic (#13023)

Signed-off-by: Emmanuel Ferdman <emmanuelferdman@gmail.com>

* Properly parse json options for key generation in the UI (#12989)

* fix: correct CompletionRequest messages type to match OpenAI API spec (#12980)

* fix: correct CompletionRequest messages type to match OpenAI API spec

- Changed messages field type from List[str] to List[ChatCompletionMessageParam]
- This ensures proper OpenAI API compatibility where messages should be objects with role and content fields
- Fixes type inconsistency in completion request handling

* feat(tests): Add comprehensive tests for CompletionRequest model

- Add test_completion.py for litellm.types.completion module
- Test ChatCompletionMessageParam type validation
- Test tool message format compatibility
- Test function message format (deprecated)
- Test multimodal content (text + image)
- Test default empty messages list
- Test all optional parameters
- Validate OpenAI ChatCompletion API message format compatibility

* chore: Improve docs for cost tracking (#12976)

* feat(langfuse-otel): Add comprehensive metadata support to Langfuse OpenTelemetry integration (#12956)

* feat(langfuse-otel): Add comprehensive metadata support to Langfuse OpenTelemetry integration

This commit brings the langfuse_otel integration to feature parity with the vanilla Langfuse integration by adding support for all metadata fields.

Changes:
- Extended LangfuseSpanAttributes enum with all supported metadata fields:
  - Generation-level: generation_name, generation_id, parent_observation_id, version, mask_input/output
  - Trace-level: trace_user_id, session_id, tags, trace_name, trace_id, trace_metadata, trace_version, trace_release, existing_trace_id, update_trace_keys
  - Debug: debug_langfuse

- Implemented metadata extraction and mapping in langfuse_otel.py:
  - Added _extract_langfuse_metadata() helper to extract metadata from kwargs
  - Support for header-based metadata (langfuse_* headers) via proxy
  - Enhanced _set_langfuse_specific_attributes() to map all metadata to OTEL attributes
  - JSON serialization for complex types (lists, dicts) for OTEL compatibility

- Updated documentation:
  - Added 'Metadata Support' section explaining all fields are now supported
  - Provided usage example showing how to pass metadata
  - Clarified that traces are viewed in Langfuse UI (not generic OTEL backends)
  - Added opentelemetry-exporter-otlp to required dependencies

This allows users to pass metadata like:
metadata={
    'generation_name': 'my-generation',
    'trace_id': 'trace-123',
    'session_id': 'session-456',
    'tags': ['prod', 'v1'],
    'trace_metadata': {'user_type': 'premium'}
}

All metadata is exported as OpenTelemetry span attributes with 'langfuse.*' prefix for easy filtering and analysis in the Langfuse UI.

* Fix ruff linting error

* test(langfuse-otel): Fix failing test and add comprehensive metadata tests

- Fix test_set_langfuse_environment_attribute to use positional arguments
  instead of keyword arguments when asserting safe_set_attribute calls
- Add test_extract_langfuse_metadata_basic to verify metadata extraction
  from litellm_params
- Add test_extract_langfuse_metadata_with_header_enrichment to test
  integration with header-based metadata using a stubbed LangFuseLogger
- Add test_set_langfuse_specific_attributes_full_mapping to comprehensively
  test all metadata field mappings and JSON serialization of complex types

These tests ensure full coverage of the langfuse_otel metadata features
added in commit ab1dbe355 and fix the CI test failure.

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>

---------

Co-authored-by: Claude <noreply@anthropic.com>

* fix scrolling issue (#13015)

* [MCP gateway] add url namespacing docs (#13063)

* added the url docs

* Added url change

* test: skip dbrx claude 3-7 sonnet test - rate limit errors

* [Bug Fix] The model gemini-2.5-flash with the merge_reasoning_content_in_choices parameter does not work (#13066)

* _optional_combine_thinking_block_in_choices

* test_optional_combine_thinking_block_with_none_content

* test: remove o1-preview

* bump: version 1.74.9 → 1.74.10

* [Feat] Add Google AI Studio Imagen4 model family  (#13065)

* add gemini

* add init files

* add get_gemini_image_generation_config

* refactor transform

* TestGoogleImageGen

* fix transform

* fix transform

* add gemini_image_cost_calculator

* add cost tracking for gemini/imagen models

* docs image gen

* docs image gen

* test_get_model_info_gemini

* default to 7 days (#12917)

* Added handling for pwd protected cert files in AOAI CertificateCredential auth (#12995)

* docs: add Qwen Code CLI tutorial (#12915)

- Add new tutorial for integrating Qwen Code CLI with LiteLLM Proxy
- Update sidebar to include Qwen Code CLI in both AI Tools and main Tutorials sections
- Document environment variables for OpenAI-compatible configuration
- Include examples for routing to various providers (Anthropic, OpenAI, Bedrock)

* docs

* Azure `api_version="preview"` support + Bedrock cost tracking via Anthropic `/v1/messages`  (#13072)

* fix(azure/chat/gpt_transformation.py): support api_version="preview"

Fixes https://github.com/BerriAI/litellm/issues/12945

* Fix anthropic passthrough logging handler model fallback for streaming requests (#13022)

* fix: anthropic passthrough logging handler model fallback for streaming requests

- Add fallback logic to retrieve model from logging_obj.model_call_details when request_body.model is empty
- Fixes issue #12933 where streaming requests to anthropic passthrough endpoints would crash due to missing model field
- Ensures downstream logging and cost calculation work correctly for all streaming scenarios
- Maintains backwards compatibility with existing non-streaming requests

* test: add minimal tests for anthropic passthrough logging handler model fallback

- Add unit tests for the model fallback logic in _handle_logging_anthropic_collected_chunks
- Test existing behavior when request_body.model is present
- Test fallback logic when request_body.model is empty but logging_obj.model_call_details has model
- Test edge cases where both sources are empty or missing
- Ensure backwards compatibility and graceful degradation

* fix(anthropic_passthrough_logging_handler.py): add provider to model name (accurate cost tracking)

* fix(anthropic_passthrough_logging_handler.py): don't reset custom llm provider, if already set

* fix: fix check

---------

Co-authored-by: Haggai Shachar <haggai.shachar@backline.ai>

* Remove extraneous `s` in docs (#13079)

* Fix list team v2 security check (#13094)

* Fix security vulnerability in list_team_v2 endpoint

- Add missing allowed_route_check_inside_route security check to list_team_v2
- Add @management_endpoint_wrapper decorator for consistency with list_team
- Add comprehensive tests to verify security checks work correctly
- Ensure non-admin users can only query their own teams
- Ensure admin users can query all teams

This fixes a security bug where non-admin users could potentially access
team information they shouldn't have access to through the list_team_v2
endpoint, which was missing the authorization check present in list_team.

* Fix test

* Test fixes

* Fixed test

* Restored invalid delete

* Revert

---------

Co-authored-by: openhands <openhands@all-hands.dev>

* [MCP gateway] add pre and during call hooks init (#13067)

* add hook init

* add during hook

* added logging

* fix: improve MCP server URL validation to support internal/Kubernetes URLs (#13099)

* fix: improve MCP server URL validation to support internal/Kubernetes URLs

- Replace strict Ant Design URL validator with flexible custom validator
- Allow URLs like http://service-name.domain.svc.cluster.:1234/mcp
- Update both create and edit MCP server forms for consistency

* refactor: extract MCP server validation into reusable utilities

- Move URL validation logic to utils.tsx to follow DRY principles
- Add validateMCPServerUrl function for flexible URL validation
- Add validateMCPServerName function for hyphen validation
- Update both create and edit components to use shared utilities
- Reduces code duplication and improves maintainability

* [Bug Fix] Gemini-CLI - The Gemini Custom API request has an incorrect authorization format (#13098)

* fix GoogleGenAIConfig

* fix validate_environment

* test_agenerate_content_x_goog_api_key_header

* set default value for mcp namespace tool name to prevent duplicate entry in table (#12894)

* [Feat] Allow using query_params for setting API Key for generateContent routes (#13100)

* fix is_generate_content_route

* fix route checks

* fix get_api_key

* add openrouter grok4 (#13018)

* docs AZURE_CERTIFICATE_PASSWORD

* fix mcp dep for litellm (#13102)

* fix: always use choice index=0 for Anthropic streaming responses (#12666)

- Fixed 'missing finish_reason for choice 1' error with reasoning_effort
- Anthropic sends multiple content blocks with different indices
- OpenAI expects all content in a single choice at index=0
- Added comprehensive tests for text-only, text+tool, and multiple tools

* BUGFIX: Jitter should be added not multiplied (#12877) (#12901)

* Jitter should be added not multiplied

This fixes a bug mentioned in https://github.com/BerriAI/litellm/issues/12877

`JITTER=0.75` is multiplied by `random.random()` so `sleep_seconds*jitter` is a tiny number that is always less than `min_timeout`.

jitter should be added not multiplied

* Add jitter to min_timeout case also

* Cleanup jitter logic

* Always apply jitter

* fix: best practices suggest this to set to true (#12809)

The order of the specification is important here, k8s will take the last value as truth. Push down to be sure schema update is done by migration job

* fix: Set user from token user_id for OpenMeter integration (#13029)

* Revert "fix: Set user from token user_id for OpenMeter integration (#13029)" (#13107)

This reverts commit f8c09e44f6c8a2e8b8f05b193d98cc7f3cdc09c8.

* Fix fallback delete (#12606)

* fix fallbacks deletion

* x

* Fix/gemini api key environment variable support (#12507)

* Fix: Add support for GOOGLE_API_KEY environment variables for Gemini API authentication

* added test cases

* incoperated feedback to make it more maintainable

* fix failed linting CI

* [MCP Gateway] Add protocol headers (#13062)

* Add protocol headers

* fix mypy

* fix tests

* fix tests

* Fix token counter to ignore unsupported keys like prefix (#11791) (#11954)

* Custom Auth - bubble up custom exceptions  (#13093)

* fix(enterprise/litellm_enterprise/proxy/auth/user_api_key_auth.py): bubble up exception if type is ProxyException

* docs(custom_auth.md): doc on bubbling up custom exceptions

* docs(index.md): add rc docker tag

* docs(index.md): cleanup

* feat: Add dot notation support for all JWT fields (#13013)

* feat: Add dot notation support for all JWT fields

- Updated all JWT field access methods to use get_nested_value for dot notation support
- Enhanced get_team_id to properly handle team_id_default fallback with nested fields
- Added comprehensive unit tests for nested JWT field access and edge cases
- Updated documentation to reflect dot notation support across all JWT fields
- Maintains full backward compatibility with existing flat field configurations

Supported fields with dot notation:
- team_id_jwt_field, team_ids_jwt_field, user_id_jwt_field
- user_email_jwt_field, org_id_jwt_field, object_id_jwt_field
- end_user_id_jwt_field (roles_jwt_field was already supported)

Example: user_id_jwt_field: 'user.sub' accesses token['user']['sub']

* fix: Add type annotations to resolve mypy errors

- Add explicit type annotation for team_ids variable in get_team_ids_from_jwt
- Add type ignore comment for sentinel object return in get_team_id
- Resolves mypy errors while maintaining functionality

* fix: Resolve mypy type error in get_team_ids_from_jwt

- Remove explicit List[str] type annotation that conflicts with get_nested_value return type
- Simplify return logic to use 'team_ids or []' ensuring always returns List[str]
- Fixes: Incompatible types in assignment (expression has type 'list[str] | None', variable has type 'list[str]')

* fix: Add proper type annotation for team_ids variable

- Use Optional[List[str]] type annotation to satisfy mypy requirements
- Resolves: Need type annotation for 'team_ids' [var-annotated]
- Maintains functionality while ensuring type safety

* refactor: remove outdated JWT unit tests and consolidate JWT-related functionality

- Deleted the test_jwt.py file as it contained outdated and redundant tests.
- Consolidated JWT-related tests into test_handle_jwt.py for better organization and maintainability.
- Updated tests to ensure proper functionality of JWT handling, including token validation and role mapping.
- Enhanced test coverage for JWT field access and nested claims handling.

* test: add comprehensive unit tests for JWT authentication

- Introduced a new test file `test_jwt.py` containing unit tests for JWT authentication.
- Implemented tests for loading configuration with custom role names, validating tokens, and handling team tokens.
- Enhanced coverage for JWT field access, nested claims, and role-based access control.
- Added fixtures for Prisma client and public JWT key generation to support testing.
- Ensured proper handling of valid and invalid tokens, including user and team scenarios.

* revert test_handle_jwt.py

* rename file

* test: remove outdated JWT nesting tests and add new nested field access tests

- Deleted the `test_jwt_nesting.py` file as it contained outdated tests.
- Introduced new tests in `test_handle_jwt.py` to verify nested JWT field access.
- Enhanced coverage for accessing nested values using dot notation and ensured backward compatibility with flat field names.
- Added tests for handling missing nested paths and appropriate default values.
- Improved handling of metadata prefixes in nested field access.

* restore file

* [Feat] MLFlow Logging - Allow adding tags for ML Flow logging requests  (#13108)

* add mlflow tags

* fixes config

* add litellm mlflow

* test_mlflow_request_tags_functionality

* docs ML flow litellm proxy

* docs ml flow

* docs mlflow

* [LLM translation] Add support for bedrock computer use (#12948)

* Add support for bedrock computer use

* remove print

* split bedrock tools

* add hosted tools

* fix tool use

* fix tool use

* fix function calling

* fix converse transformation

* fix tests

* bump: version 1.74.10 → 1.74.11

* transform_image_generation_response

* fix transform_image_generation_response

* Revert "[MCP Gateway] Add protocol headers (#13062)"

This reverts commit 8de24bab7c3ba14c94ad34d270315f1050e693d8.

* fix test_mlflow_request_tags_functionality

* After selecting date range show loader on usage cost charts (#13113)

* prettier

* added loader in bar chart

* prettier

* added existing loader style

* make datepicker responsive

* test_user_api_key_auth

* Revert "Revert "[MCP Gateway] Add protocol headers (#13062)""

This reverts commit acd915f2dbea03a5c44c7502096a1edc4acd0a3b.

* use _safe_get_request_query_params

* test: update test

* Revert "[LLM translation] Add support for bedrock computer use (#12948)" (#13118)

This reverts commit 760d747465d9d6a07d711c04d83b136a6e285dd6.

* test: update test

* fix(model_checks.py): handle custom values in wildcard model name (e.g. genai/test/*) (#13116)

Fixes https://github.com/BerriAI/litellm/issues/13078

* move to use_prisma_migrate by default + resolve team-only models on auth checks + UI - add sagemaker on UI (#13117)

* fix(proxy_cli.py): make use_prisma_migrate proxy default

Fixes https://github.com/BerriAI/litellm/issues/13046

 Prisma migrate deploy prevents resetting db

* fix(auth_checks.py): resolve team only models while doing auth checks on model access groups

Fixes issue where key had access via an access group, but team only model could not be called

* test(test_router.py): add unit testing

* feat(provider_specific_fields.tsx): add aws sagemaker on UI

* test: update test

* fix tool aws bedrock call index when the function only have optional arg (#13115)

* docs: cleanup

* [MCP Gateway] add health check endpoints for MCP (#13106)

* add health check endpoints for MCP

* add import

* Clean up endpopints

* fix ruff

* [MCP Protocol header] fix issue with clients protocol header (#13112)

* fix headers

* fix test

* fix ruff

* fix mypy

* Added Voyage, Jinai, Deepinfra and VolcEngine providers on the UI (#13131)

* added voyage and jinai and volcengine

* deepinfra added and alphabetically ordered

* docs: cleanup

* fix object permission for orgs (#13142)

* New Advanced Date Range Picker Component (#13141)

* new date-range picker added

* remove unused utils

* [Feat] UI + Backend add a tab for use agent activity  (#13146)

* Add user agent analytics endpoints and UI for tracking client metrics

Co-authored-by: ishaan <ishaan@berri.ai>

* fix user agent analytics

* fix getting DAU

* fixes for user agent

* showing top user agents

* on this page remove Success Rate by User Agent

* fix linting

* add agent activity

* cleanup interface

* fix ruff

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: ishaan <ishaan@berri.ai>

* [LLM translation] Fix bedrock computer use (#13143)

* Add support for bedrock computer use

* remove print

* split bedrock tools

* add hosted tools

* fix tool use

* fix tool use

* fix function calling

* fix converse transformation

* fix tests

* fix llm translation test

* fix computer use

* [MCP Guardrails] move pre and during hooks to ProxyLoggin (#13109)

* move pre and during hooks t o ProxyLoggin

* fix lint

* fix ruff

* fix tests

* [Feat] v2 updates - tracking DAU, WAU, MAU for coding tool usage + show Daily Usage per User (#13147)

* Add user agent analytics endpoints and UI for tracking client metrics

Co-authored-by: ishaan <ishaan@berri.ai>

* fix user agent analytics

* fix getting DAU

* fixes for user agent

* showing top user agents

* on this page remove Success Rate by User Agent

* fix linting

* add agent activity

* cleanup interface

* fix ruff

* round cost

* fix charts

* fixes - show DAU, MAU, WAU

* move to a diff file

* fix

* fixes for user agent analytics

* fix user_agent_analytics_endpoints

* fix mypy linting

* fix linting

---------

Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: ishaan <ishaan@berri.ai>

* Litellm explore postgres db ci cd (#13156)

* ci(config.yml): testing with ci/cd db

* build: spin up pg db in ci/cd test

* [MCP Gateway] Litellm mcp client list fail (#13114)

* fix headers

* fix test

* fix ruff

* added try except for catching errors which lead to client failures

* fix mypy

* fix ruff

* fix tests

* fix python error

* fix test

* fix test

* fixed the MCP Call Tool result

* ci: remove bad script

* ci(config.yml): run prisma generate before running enterprise tests

* fix grype scan

* build(pyproject.toml): bump version

* ci: migrate to db in pipeline

* fix migrations (#13157)

* Revert "[LLM translation] Fix bedrock computer use (#13143)"

This reverts commit 840dd2e7c7812a2967890593e24de06c1f658adb.

* poetry lock

* test: handle api instability

* ci(config.yml): remove check

* ci: migrate to postgres in ci/cd

* test fix xai - it goes through base llm tests already

* build(config.yml): migrate build_and_test to ci/cd pg db (#13166)

* add framework name to UserAgent header in AWS Bedrock API call (#13159)

* fix: remove obsolete attribute `version` in docker compose (#13172)

Fix the warning: WARN[0000] docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion

* test_chat_completion_ratelimit

* Revert "add framework name to UserAgent header in AWS Bedrock API call (#13159)"

This reverts commit 77f506e860654252797f0f555c55f166cd04762c.

* [Feat] Background Health Checks - Allow disabling background health checks for a specific  (#13186)

* disable background health checks for specific models

* test_background_health_check_skip_disabled_models

* Disable Background Health Checks For Specific Models

* [Proxy Startup]fix db config through envs (#13111)

* fix db config through envs

* add helper

* fix ruff

* fix imports

* add unit tests in db config changes

* UI - new build

* fix: support negative indexes in cache_control_injection_points for Anthropic Claude (#10226) (#13187)

* [Bug Fix] Gemini-CLI Integration - ensure tool calling works as expected on generateContent (#13189)

* transform_generate_content_request

* add tools in GenerateContentRequestDict

* add generate_content_handler tool calling

* google_generate_content_endpoint_testing

* test_mock_stream_generate_content_with_tools

* test_validate_post_request_parameters

* fixes for generate_content_handler

* fix VertexAIGoogleGenAIConfig

* fixes veretx ai

* google_generate_content_endpoint_testing

* test_async_streaming_with_logging

* load_vertex_ai_credentials

* test_vertex_anthropic.py

* [Bug Fix] Infra - ensure that stale Prisma clients disconnect DB connection  (#13140)

* ensure original client is disconnected when re-creating

* test_recreate_prisma_client_successful_disconnect

* test_recreate_prisma_client_successful_disconnect

* [Feat] Allow redacting message / response content for specific logging integrations - DD LLM Observability (#13158)

* fix redact_standard_logging_payload

* add StandardCustomLoggerInitParams

* allow defining DatadogLLMObsInitParams

* fix init DataDogLLMObsLogger

* fix import

* update redact_standard_logging_payload_from_model_call_details

* test_dd_llms_obs_redaction

* docs DD logging

* docs DD

* docs DD

* Redacting Messages, Response docs DD LLM Obs

* fix redaction logic

* fix create_llm_obs_payload

* fix logging response

* fixes

* ruff fix

* fix test

* test_dd_llms_obs_redaction

* test_create_llm_obs_payload

* redact_standard_logging_payload_from_model_call_details

* img - dd_llm_obs

* docs DD

* fix linting

* fix linting

* fix mypy

* test_create_llm_obs_payload

* test_create_llm_obs_payload

* fix mock_env_vars

* fix _handle_anthropic_messages_response_logging

* Litellm fix fallbacks UI (#13191)

* UI - fix setting fallbacks on UI

* fix add fallbacks

* ui polish

* fix: correct patch path in langfuse test for MAX_LANGFUSE_INITIALIZED_CLIENTS (#13192)

The test was failing because it was trying to patch MAX_LANGFUSE_INITIALIZED_CLIENTS
at the wrong path. The constant is imported from litellm.constants into the langfuse
module namespace, so we need to use patch.object on the imported module reference.

Changes:
- Import langfuse module explicitly for patching
- Use patch.object instead of patch string path
- This fixes the AttributeError that was causing CI failures

* ui new build

* add When to Use Each Endpoint (#13193)

* Fix - using managed files w/ OTEL + UI - add model group alias on UI (#13171)

* fix(router.py): safe deep copy kwargs

OTEL adds a parent_otel_span which cannot be deepcopied

* fix: use safe deep copy in other places as well

* test: add script to check and ban copy.deepcopy of kwargs

enforce safe_deep_copy usage

* build(ui/): new component for adding model group alias on UI

* fix(proxy_server.py): support updating model_group_alias via /config/update

allows ui component to work

* fix(router.py): update model_group_alias in router settings based on db value

* fix: fix code qa error

* Anthropic - working mid-stream fallbacks  (#13149)

* fix(router.py): add acompletion_streaming_iterator inside router

allows router to catch errors mid-stream for fallbacks

Work for https://github.com/BerriAI/litellm/issues/6532

* fix(router.py): working mid-stream fallbacks

* fix(router.py): more iterations

* fix(router.py): working mid-stream fallbacks with fallbacks set on router

* fix(router.py): pass prior content back in new request as assistant prefix message

* fix(router.py): add a system prompt to help guide non-prefix supporting models to use the continued text correctly

* fix(common_utils.py): support converting `prefix: true` for non-prefix supporting models

* fix: reduce LOC in function

* test(test_router.py): add unit tests for new function

* test: add basic unit test

* fix(router.py): ensure return type of fallback stream is compatible with CustomStreamWrapper

prevent client code from breaking

* fix: cleanup

* test: update test

* fix: fix linting error

* Anthropic - mid stream fallbacks p2 (add token usage across both calls) (#13170)

* fix(router.py): add acompletion_streaming_iterator inside router

allows router to catch errors mid-stream for fallbacks

Work for https://github.com/BerriAI/litellm/issues/6532

* fix(router.py): working mid-stream fallbacks

* fix(router.py): more iterations

* fix(router.py): working mid-stream fallbacks with fallbacks set on router

* fix(router.py): pass prior content back in new request as assistant prefix message

* fix(router.py): add a system prompt to help guide non-prefix supporting models to use the continued text correctly

* fix(common_utils.py): support converting `prefix: true` for non-prefix supporting models

* fix: reduce LOC in function

* test(test_router.py): add unit tests for new function

* test: add basic unit test

* fix(router.py): ensure return type of fallback stream is compatible with CustomStreamWrapper

prevent client code from breaking

* fix: cleanup

* test: update test

* fix: fix linting error

* fix(router.py): return combined usage

ensures accurate usage tracking on clientside for stream w/ fallbacks

* [UI QA] QA - Agent Activity Tab  (#13203)

* backend fixes

* fixes for User-Agent ui

* UI fixes chart loader

* fixes chart loader

* fixes ChartLoader

* fix ChartLoader

* fixes for analytics

* Fix/panw prisma airs post call hook (#13185)

* fix(guardrails): Fix PANW Prisma AIRS post-call hook method name

- Changed async_post_call_hook to async_post_call_success_hook to match proxy calling convention
- Added event_hook parameter to initialization to ensure proper hook registration
- Fixes post-call response scanning for PANW Prisma AIRS guardrails

Resolves issue where post-call hooks were not being invoked due to method name mismatch.

* Update PANW Prisma AIRS tests to use correct method name

* allow helm hooks for migrations job (#13174)

* add openssl in apk install in runtime stage in dockerfile.non_root (#13168)

* add openssl in apk install in runtime stage in dockerfile.non_rootdocker-compose logs -f litellm

* Improve Docker-compose.yaml for local debugging

---------

Co-authored-by: Krish Dholakia <krrishdholakia@gmail.com>

* bump: version 1.74.12 → 1.74.13

* bump: version 1.74.13 → 1.74.14

* Prompt Management - add local dotprompt file support

* allow redifine base api url in pass trough (#13134)

* Fix API Key Being Logged (#12978)

* AIF-262 Fix for keys being logged

* AIF-262 Undid http exception detail removal

* AIF-262 Converted f-string to normal

* build(config.yml): use ci/cd postgres in test

* fix(litellm_logging.py): fix merge error

* test: update test

* test: update tests

* test: update tests

* test: loosen check

* build(ui/): fix linting errors

* fix(model_group_alias_settings.tsx): fix check

* test: remove bad unit tests

* test: update unit tests

* docs(index.md): cleanup

* Index.md - cleanup docs (#13215)

* docs: add highlights

* docs(index.md): add model-level guardrails

* docs(index.md): cleanup notes

* docs: fix docs

* docs: add more details

* docs(index.md): cleanup doc

* [LLM translation] Fix bedrock computer use #13143 (#13150)

* fix json test

* fix pr

* fix bedrock computer use tool

* added unit test

* fix failing prisma tesT

* fix prisma connect

* docs(index.md): cleanup

* [QA] Viewing Agent Activity Headers on UI Usage Page (#13212)

* qa - agents

* refactored WAU, MAU and DAU endpoints

* fixes for dau, wau, mau

* use stack=true

* fixes for DAU calc

* fixes for rendering WAU, MAU

* use 1 section for topline

* Fixes for endpoint

* remove filter

* fix spacing

* fix activity

* working UI rendering

* fixes for chart data

* allow selecting specific tags

* add DistinctTagResponse endpoints

* use wide selector

* add types

* fixes for UI rendering

* get_per_user_analytics

* test_recreate_prisma_client_successful_disconnect

* ui new build

* fix vertex deprecated old model

* [Separate Health App] Update Helm Deployment.yaml (#13162)

* add helm deployment fix

* clean deployment

* [Proxy]fix key mgmt (#13148)

* fix key mgmt

* Add unit test

* [LLM] fix model reload on model update (#13216)

* fix model reload on model update

* remove the flag

* suppress httpx logging (#13217)

* [MCP Gateway] Litellm mcp pre and during guardrails (#13188)

* add guardrail support

* add guardrail support

* guardrails for MCP

* added changes

* add mcp guardrails

* added test

* add ui

* fix guardrail form

* working with cursor

* remvoe print

* fix mcp servertests

* fix mypy and remove console logs

* fix mypy and remove console logs

* fix mypy tests

* testing fixes - vertex ai deprecated claude 3 sonnet models

* Add advanced date picker to all the tabs on the usage page (#13221)

* advancedatepicker for tag usage and team usage

* reduce white space in date picker

* selected time range option is visible

* dont wait for apply button to select relative time options

* add Perplexity citation annotations support (#13225)

* fix: role chaining and session name with webauthentication for aws bedrock (#13205)

* fix(bedrock): prevent duplicate role assumption in EKS/IRSA environments

Fixes issue where AWS role assumption would fail in EKS/IRSA environments
when trying to assume the same role that's already being used.

The problem occurred when:
1. EKS/IRSA automatically assumes a role (e.g., LitellmRole)
2. LiteLLM tries to assume the same role again, causing AccessDenied errors
3. Different models with different roles would fail due to incorrect role context

Changes:
- Added check in _auth_with_aws_role() to detect if already using target role
- Skip role assumption if current identity matches target role
- Return current credentials instead of attempting duplicate assumption
- Added comprehensive test coverage for the fix

This ensures proper role chaining works in EKS/IRSA environments where:
- Service Account can assume Role A
- Role A can assume Role B for different models/accounts

Resolves the AccessDenied errors reported in bedrock usage scenarios.

* fix(bedrock): simplify role assumption for EKS/IRSA environments

Fixes AWS Bedrock role assumption in EKS/IRSA environments by properly
handling ambient credentials when no explicit credentials are provided.

The issue occurred because commit 197e7efa8f097bb935cf86dc4100422487a40955
introduced changes that broke role assumption in EKS/IRSA environments.

Changes:
- Simplified _auth_with_aws_role() to use ambient credentials when no
  explicit AWS credentials are provided (aws_access_key_id and
  aws_secret_access_key are both None)
- This allows web identity tokens in EKS/IRSA to work automatically
  through boto3's credential chain
- Maintains backward compatibility for explicit credential scenarios

Added comprehensive test coverage:
- test_eks_irsa_ambient_credentials_used: Verifies ambient credentials work
- test_explicit_credentials_used_when_provided: Ensures explicit creds still work
- test_partial_credentials_still_use_ambient: Edge case handling
- test_cross_account_role_assumption: Multi-account scenarios
- test_role_assumption_with_custom_session_name: Custom session names
- test_role_assumption_ttl_calculation: TTL calculation verification
- test_role_assumption_error_handling: Error propagation
- test_multiple_role_assumptions_in_sequence: Sequential role assumptions

This fix ensures that in EKS/IRSA environments:
1. Service accounts can assume their initial role via web identity
2. That role can then assume other roles across accounts as configured
3. Different models can use different roles without conflicts

* fix(bedrock): add automatic IRSA detection for EKS environments

- Detect AWS_WEB_IDENTITY_TOKEN_FILE and AWS_ROLE_ARN environment variables
- Automatically use web identity token flow when IRSA is detected
- Read web identity token from file and pass to existing auth method
- Add test coverage for IRSA environment detection
- Fixes authentication errors in EKS with IRSA when no explicit credentials provided

* fix(bedrock): skip role assumption when IRSA role matches requested role

- Detect when AWS_ROLE_ARN environment variable matches the requested role
- Skip unnecessary role assumption when already running as the target role
- Use existing env vars authentication method for IRSA credentials
- Add test coverage for same-role IRSA scenario
- Fixes 'not authorized to perform: sts:AssumeRole' errors when trying to assume the same role

* fix(bedrock): use boto3's native IRSA support for cross-account role assumption

- Replace custom web identity token handling with boto3's built-in IRSA support
- boto3 automatically reads AWS_WEB_IDENTITY_TOKEN_FILE and assumes initial role
- Then use standard assume_role for cross-account access
- Update test to mock boto3 STS client instead of internal methods
- Fixes 'OIDC token could not be retrieved from secret manager' error

* fix(bedrock): improve IRSA error handling and add debug logging

- Add debug logging to show current identity and role assumption attempts
- Provide clearer error messages for trust policy issues
- Fix region handling in IRSA flow
- Re-raise exceptions instead of silently falling through
- This helps diagnose cross-account role assumption permission issues

* fix(bedrock): manually assume IRSA role with correct session name for cross-account scenarios

- When doing cross-account role assumption, manually assume the IRSA role first with the desired session name
- This ensures the session name in the assumed role ARN matches what's expected in trust policies
- For same-account scenarios, continue using boto3's automatic IRSA support
- Updated tests to handle the new flow
- This fixes the issue where cross-account trust policies require specific session names

* fix: Fix linting issues in base_aws_llm.py

- Fix f-string without placeholders (F541)
- Refactor _auth_with_aws_role to reduce statements count (PLR0915)
  - Extract _handle_irsa_cross_account helper method
  - Extract _handle_irsa_same_account helper method
  - Extract _extract_credentials_and_ttl helper method

---------

Co-authored-by: openhands <openhands@all-hands.dev>

* Fix missing extra_headers support for vLLM/openai_like embeddings (#13198)

- Add extra_headers handling to hosted_vllm/openai_like embedding providers
- Matches existing pattern used in OpenAI embeddings section
- Fixes issue where custom headers were dropped for vLLM embedding requests

Fixes #13088

* litellm/proxy: preserve model order of /v1/models and /model_group/info (#13178)

Closes #12644

Signed-off-by: Alexander Yastrebov <alexander.yastrebov@zalando.de>

* Prompt Management - abstract prompt templates away from model list (enables permission management on prompt templates)  (#13219)

* feat: initial commit with prompt management support on pre-call hooks

allows prompt templates to work before assigning specific models

* feat: initial logic for independent prompt management settings

* feat(proxy_server.py): working logic for loading in the prompt templates from config yaml

allows creating an independent 'prompts' section in the config yaml

* feat(prompt_registry.py): working e2e custom prompt templates with guardrails and models

* refactor(prompts/): move folder inside proxy folder

easier management for prompt endpoints

* fix: fix linting error

* fix: fix check

* [QA Fixes for MCP] - Ensure MCPs load + don't run a health check everytime we load MCPs on UI (#13228)

* qa - mcps should load even if they don't have required fields

* fix loading MCPs

* Revert "fix: role chaining and session name with webauthentication for aws be…" (#13230)

This reverts commit 0ac093b59edab48b1400bc84e133a35ef4accfa2.

* fix(proxy_setting_endpoints.py): don't block startup if team doesn't exist in default team member budget

* Prompt Management (2/2) - New `/prompt/list` endpoint + key-based access to prompt templates (#13218)

* feat: initial commit with prompt management support on pre-call hooks

allows prompt templates to work before assigning specific models

* feat: initial logic for independent prompt management settings

* feat(proxy_server.py): working logic for loading in the prompt templates from config yaml

allows creating an independent 'prompts' section in the config yaml

* feat(prompt_registry.py): working e2e custom prompt templates with guardrails and models

* refactor(prompts/): move folder inside proxy folder

easier management for prompt endpoints

* feat(prompt_endpoints.py): working `/prompt/list` endpoint

returns all available prompts on proxy

* feat(key_management_endpoints.py): support storing 'prompts' in key metadata

allows giving keys access to specific prompts

* feat(prompt_endpoints.py): enable key-based access to /prompts/list

ensures key can only see prompts it has access to

* fix(init_prompts.py): fix linting error

* fix: fix ruff check

* fix(proxy/_types.py): add 'prompts' to newteamrequest

* fix(litellm_logging.py): update logged message with scrubbed value

* truncateUserAgent

* [UI QA Fixes] Stable release (#13231)

* qa - user agent view

* fixes for usage time selector

* Revert "Fix SSO Logout | Create Unified Login Page with SSO and Username/Password Options (#12703)"

This reverts commit a752d7acc9f9db145d0b1d49ddb53263b67d0b31.

* Revert "Revert "Fix SSO Logout | Create Unified Login Page with SSO and Username/Password Options (#12703)""

This reverts commit 5fe37b6f72060add859a22ddda0665cd1635f98f.

* fixes - ui login with SSO

* doc fix - missing "prompts" in /key endpoint swagger

* ui new build

* bump: version 1.74.14 → 1.74.15

* ruff fix

* docs release notes

* fixes MCP gateway docs

* [docs release notes] (#13237)

* docs release notes

* docs release notes

* docs rnotes

* docs api version

* fixes docs

* docs rn

* docs computer use

* docs RC

* docs - Track Usage for Coding Tools

* docs cost tracking coding

* agent 4.png

* docs fix

* docs fix

* docs fix

* docs User Agent Activity Tracking

* UI - Add giving keys prompt access (#13233)

* fix(create_key_button.tsx): add prompts on UI

* feat(key_management_endpoints.py): support adding prompt to key via `/key/update`

* fix(key_info_view.tsx): show existing prompts on key in key_info_view.tsx

* fix(key_edit_view.tsx): UX - disable premium feature for non-premium users

prevent accidental clicking

* fix(create_key_button.tsx): disable premium features behind flag, prevent errors

* fix(key_management_endpoints.py): fix key update logic \

* fix: fix check

* docs: document new params

* Prompt Management - Add table + prompt info page to UI  (#13232)

* fix(create_key_button.tsx): add prompts on UI

* feat(key_management_endpoints.py): support adding prompt to key via `/key/update`

* fix(key_info_view.tsx): show existing prompts on key in key_info_view.tsx

* fix(key_edit_view.tsx): UX - disable premium feature for non-premium users

prevent accidental clicking

* fix(create_key_button.tsx): disable premium features behind flag, prevent errors

* feat(prompts.tsx): add new ui component to view created prompts

enables viewing prompts created on config

* feat(prompt_info.tsx): add component for viewing the prompt information

* Prompt Management - add prompts on UI  (#13240)

* fix(create_key_button.tsx): add prompts on UI

* feat(key_management_endpoints.py): support adding prompt to key via `/key/update`

* fix(key_info_view.tsx): show existing prompts on key in key_info_view.tsx

* fix(key_edit_view.tsx): UX - disable premium feature for non-premium users

prevent accidental clicking

* fix(create_key_button.tsx): disable premium features behind flag, prevent errors

* feat(prompts.tsx): add new ui component to view created prompts

enables viewing prompts created on config

* feat(prompt_info.tsx): add component for viewing the prompt information

* feat(prompt_endpoints.py): support converting dotprompt to json structure + accept json structure in promptmanager

allows prompt manager to work with api endpoints

* test(test_prompt_manager.py): add unit tests for json data input

* feat(dotprompt/__init__.py): add prompt data to dotpromptmanager

* fix(prompt_endpoints.py): working crud endpoints for prompt management

* feat(prompts/): support `prompt_file` for dotprompt

allows to precisely point to the prompt file a prompt should use

* feat(proxy/utils.py): resolve prompt id correctly

resolves user sent prompt id with internal prompt id

* feat(schema.prisma): initial pr with db schema for prompt management table

allows post endpoints to work with backend

* feat(prompt_endpoints.py): use db in patch_prompt endpoint

* feat(prompt_endpoints.py): use db for update_prompt endpoint

* feat(prompt_endpoints.py): use db on prompt delete endpoint

* build(schema.prisma): add prompt tale to schema.prisma in litellm-proxy-extras

* build(migration.sql): add new sql migration file

* fix(init_prompts.py): fix init

* feat(prompt_info_view.tsx): show the raw prompt template on ui

allows developer to know the prompt template they'll be calling

* feat(add_prompt_form.tsx): working ui add prompt flow

allows user to add prompts to litellm via ui

* build(ui/): styling fixes

* build(ui/): prompts.tsx

styling improvements

* fix(add_prompt_form.tsx): styling improvements

* build(prompts.tsx): styling improvements

* build(ui/): styling improvements

* build(ui/): fix ui error

* fix: fix ruff check

* docs: document new api params

* test: update tests

* fix openshift (#13239)

* build: update poetry

* fix(key_management_endpoints.py): fix check

* docs(index.md): cleanup

* [LLM Translation] Fix Model Usage not having text tokens (#13234)

* fix + test

* remove test comments

* fix mypy

* fix mypy

* fix tests

* [UI] Add team deletion check for teams with keys (#12953)

* added check option

* Add underline

* make less verbosE

* [Bug Fix] OpenAI / Azure Responses API - Add `service_tier` , `safety_identifier` supported params (#13258)

* test_aresponses_service_tier_and_safety_identifier

* add service_tier + safety_identifier

* fix get_supported_openai_params

* add safety_identifier + service_tier for responses()

* Bug Fix - Responses API raises error with Gemini Tool Calls in `input` (#13260)

* add _transform_responses_api_function_call_to_chat_completion_message

* test_responses_api_with_tool_calls

* TestFunctionCallTransformation

* fixes for responses API testing google ai studio

* TestGoogleAIStudioResponsesAPITest

* test_responses_api_with_tool_calls

* test_responses_api_with_tool_calls

* test_basic_openai_responses_streaming_delete_endpoint

* docs(index.md): cleanup tag

* docs(user_keys.md): add litellm python sdk tab

* Update model_prices_and_context_window.json (#13244)

* [Bug Fix] Fix  Server root path regression on UI when using "Login" (#13267)

* bug fix serve_login_page

* test_serve_login_page_server_root_path

* Support OCI provider (#13206)

* create OCI required files

* request and response conversion for non-streaming chat

* support tool calling with OCI generic API without streaming

* adaptation of api call for generic and cohere format

* include tool calls and responses in generic api and dropping support for cohere

* fix invalid content-length error

* support streaming for generic api

* fix auth error when using acompletion with streaming

* refactor: use base_llm_http_handler and include API type definitions

* update types and add type safety in different methods

* fix OCIFunction format

* create custom stream wrapper for decoding OCI stream

* remove unused files

* create unit tests for OCI

* lint the code

* remove manual test

* docs: update the docs to include OCI

* Add GCS bucket caching support (#13122)

* Fix: Langfuse reporting "client closed" error due to httpx client TTL (#13045)

* Fix: Langfuse reporting "client closed" error due to httpx client TTL

* remove log

* add correct pricing (#13269)

* refactor(oci/chat/transformation.py): lazy load package imports

* [Bug Fix] Prometheus - fix for `litellm_input_tokens_metric`, `litellm_output_tokens_metric`  - Note this updates the metric name  (#13271)

* fixes for litellm_tokens_metric

* test_prometheus_token_metrics_with_prometheus_config

* bump: version 1.74.15 → 1.75.0

* bump: version 1.75.0 → 1.75.1

* add litellm-enterprise==0.1.17

* input cost per token higher than 1 test (#13270)

* [LLM Translation] Support /v1/models/{model_id} retrieval (#13268)

* added model id endpoint

* fix test

* add route to internal users

* make the functions reusable

* fixed mypy

* [UI] - Add ability to set model alias per key/team (#13276)

* update model alias on keys

* team model aliases

* fix model aliases

* fixes for teams

* fix OCI linting errors  (#13279)

* fix(types/llms/oci.py): fix linting errors

* fix(oci.py): fix linting error

* fix(oci.py): fix linting errors

* fix: fix linting error

* fix: fix linting error

* Ensure disable_llm_api_endpoints works + Add wildcard model support for 'team-byok' model  (#13278)

* fix(route_checks.py): ensure disable llm api endpoints is correctly set

* fix(route_checks.py): raise httpexception

raise expected exceptions

* fix(router.py): handle team only wildcard models

fixes issue where team only wildcard models were not considered during auth checks

* fix(router.py): handle team only wildcard models

fixes issue where team only wildcard models were not considered during auth checks

* fix(main.py): handle tool being a pydantic object (#13274)

* fix(main.py): handle tool being a pydantic object

Fixes https://github.com/BerriAI/litellm/issues/13064

* fix(prompt_templates/common_utils.py): fix unpack defs deepcopy issue

Fixes https://github.com/BerriAI/litellm/issues/13151

* fix(utils.py): handle tools is none

* test_function_calling_with_tool_response

* Revert "Fix: Langfuse reporting "client closed" error due to httpx client TTL…" (#13291)

This reverts commit 1c6be9bdadbeac3ae66bac096e281bc305cd6b5b.

---------

Signed-off-by: Emmanuel Ferdman <emmanuelferdman@gmail.com>
Signed-off-by: Alexander Yastrebov <alexander.yastrebov@zalando.de>
Co-authored-by: Ishaan Jaff <ishaanjaffer0324@gmail.com>
Co-authored-by: Santosh Dhaladhuli <80815111+SantoshDhaladhuli@users.noreply.github.com>
Co-authored-by: Murad Khafizov <101127600+murad-khafizov@users.noreply.github.com>
Co-authored-by: Gaston <grodriguez160597@gmail.com>
Co-authored-by: Cole McIntosh <82463175+colesmcintosh@users.noreply.github.com>
Co-authored-by: sings-to-bees-on-wednesdays <222684290+sings-to-bees-on-wednesdays@users.noreply.github.com>
Co-authored-by: Dmitriy Alergant <93501479+DmitriyAlergant@users.noreply.github.com>
Co-authored-by: Krish Dholakia <krrishdholakia@gmail.com>
Co-authored-by: Jugal D. Bhatt <55304795+jugaldb@users.noreply.github.com>
Co-authored-by: Emerson Gomes <emerson.gomes@thalesgroup.com>
Co-authored-by: Matthias Dittrich <matthi.d@gmail.com>
Co-authored-by: Christoph Koehler <christoph@zerodeviation.net>
Co-authored-by: Emmanuel Ferdman <emmanuelferdman@gmail.com>
Co-authored-by: stellasec <stella@stellasec.com>
Co-authored-by: direcision <direcision@gmail.com>
Co-authored-by: Richard Tweed <RichardoC@users.noreply.github.com>
Co-authored-by: Alex Strick van Linschoten <strickvl@users.noreply.github.com>
Co-authored-by: Claude <noreply@anthropic.com>
Co-authored-by: tanjiro <56165694+NANDINI-star@users.noreply.github.com>
Co-authored-by: Felix Burmester <57833596+Ne0-1@users.noreply.github.com>
Co-authored-by: Haggai Shachar <haggai.shachar@backline.ai>
Co-authored-by: Max Rabin <927792+maxrabin@users.noreply.github.com>
Co-authored-by: Tim O'Farrell <tofarr@gmail.com>
Co-authored-by: openhands <openhands@all-hands.dev>
Co-authored-by: Shao-Kan Chu <shao.chu@mail.utoronto.ca>
Co-authored-by: Maksim <74874309+Maximgitman@users.noreply.github.com>
Co-authored-by: Pathikrit Bhowmick <pathikritbhowmick@msn.com>
Co-authored-by: Marvin Huetter <61065254+huetterma@users.noreply.github.com>
Co-authored-by: Better than breakfast. <adr.viper@gmail.com>
Co-authored-by: zengxu <zengxu_121@126.com>
Co-authored-by: Siddharth Sahu <112792547+sahusiddharth@users.noreply.github.com>
Co-authored-by: Amit Kumar <123643281+Amit-kr26@users.noreply.github.com>
Co-authored-by: Johnny.H <jnhyperion@gmail.com>
Co-authored-by: Cursor Agent <cursoragent@cursor.com>
Co-authored-by: ishaan <ishaan@berri.ai>
Co-authored-by: 0x-fang <fanggong@amazon.com>
Co-authored-by: Kowyo <kowyo@outlook.com>
Co-authored-by: Anand Khinvasara <kanand90@gmail.com>
Co-authored-by: Jason Roberts <51415896+jroberts2600@users.noreply.github.com>
Co-authored-by: unique-jakub <jakub@unique.ch>
Co-authored-by: Mateo Di Loreto <101841200+mdiloreto@users.noreply.github.com>
Co-authored-by: Dmitry Tyumentsev <56769451+tyumentsev4@users.noreply.github.com>
Co-authored-by: aayush-malviya-acquia <aayush.malviya@acquia.com>
Co-authored-by: Sameer Kankute <135028480+kankute-sameer@users.noreply.github.com>
Co-authored-by: Alexander Yastrebov <yastrebov.alex@gmail.com>
Co-authored-by: Benjamin Bartels <benjamin@bartels.dev>
Co-authored-by: breno-aumo <160534746+breno-aumo@users.noreply.github.com>
Co-authored-by: Pascal Bro <git@pascalbrokmeier.de>
Co-authored-by: Perling <linsmiling@sina.cn>
```

[fzowl](https://github.com/fzowl) pushed a commit to fzowl/litellm that referenced this pull request [ Jun 24, 2026 ](https://github.com/BerriAI/litellm/pull/12988#ref-commit-c379846)
` remove features from enterprise ([](https://github.com/fzowl/litellm/commit/c379846f176d3a2d5c6b93a7351cb6e01e491112 "remove features from enterprise \(#12988\)")BerriAI#12988[](https://github.com/BerriAI/litellm/pull/12988))[](https://github.com/fzowl/litellm/commit/c379846f176d3a2d5c6b93a7351cb6e01e491112 "remove features from enterprise \(#12988\)") `
This pull request was **closed**. 
Hide environments Show environments
### This branch was successfully deployed
1 active deployment 
Preview — `c76975dc` [Deployed](https://litellm-giusyrdxw-clerkieai.vercel.app) Jul 25, 2025 by [vercel[bot]](https://github.com/apps/vercel)
[ View deployment ](https://litellm-giusyrdxw-clerkieai.vercel.app)
[Sign up for free](https://github.com/join?source=comment-repo) **to join this conversation on GitHub**. Already have an account? [Sign in to comment](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2FBerriAI%2Flitellm%2Fpull%2F12988)
###  Labels 
None yet 
###  1 participant 
Add this suggestion to a batch that can be applied as a single commit.This suggestion is invalid because no changes were made to the code.Suggestions cannot be applied while the pull request is closed.Suggestions cannot be applied while viewing a subset of changes.Only one suggestion per line can be applied in a batch.Add this suggestion to a batch that can be applied as a single commit.Applying suggestions on deleted lines is not supported.You must change the existing code in this line in order to create a valid suggestion.Outdated suggestions cannot be applied.This suggestion has been applied or marked resolved.Suggestions cannot be applied from pending reviews.Suggestions cannot be applied on multi-line comments.Suggestions cannot be applied while the pull request is queued to merge.Suggestion cannot be applied right now. Please check back later.
You can’t perform that action at this time. 
