# OpenUSD CI Ordered Goals

Status: First draft

## Scope

- Ordered goals and dependencies only
- Follow-up work:
  - Define deliverables and success metrics
  - Assign planning horizons

## End state

- AOUSD-managed CI, continuous testing, and continuous delivery
- AOUSD-controlled repositories, runners, services
- `PixarAnimationStudios/OpenUSD` triggers with reliable status reporting
- Public unit, GPU, WebAssembly, and performance testing
- PyPI packages and downloadable binary archives
- Public interfaces to trigger and report Pixar validation that must remain
  private (I.E interal performance tests should report go/no-go)
- Implementation and maintenance by AOUSD Build Interest Group contributors
  - Optional contracted support through the Linux Foundation

## Current decisions and constraints

- Initial development:
  - Private fork: <https://github.com/AOUSD/OpenUSD>
  - Provisional branch: `aousd-ci`
- Starting point: public `PixarAnimationStudios/OpenUSD` GitHub Actions
- Workflow system: GitHub Actions unless a compelling alternative emerges
- Initial invocation: manually triggered github action
  - Takes an optional parameter to build from AOUSD/PixarAnimationStudios, tag,
    branch or SHA
  - Optionally build Pypi packages
  - Build, test and publish workflows
- Initial platforms: Linux x86-64, macOS, WebAssembly, and Windows x86-64
  - No wasm for Pypi  
- No platform priority
  - Sequence based on contributor familiarity and availability, plus
    infrastructure readiness
  - Parallel work when practical
- `usd-core` Python versions: versions supported by the corresponding OpenUSD
  release
- PyPI: wheels only; no source distributions
- Downloadable binaries: ZIP archives
- Pixar processes are happening in parallel until we are ready to switch to AOUSD CI:
  - PR validation
  - Internal releases
  - Internal performance testing
  - Proprietary-asset validation

## Ordered goals

### 1. Establish governance and inventory the current system

- Decision makers, maintainers, access model, and coordination process
- Public workflow inventory:
  - Triggers, runners, and ownership
  - Platforms, compilers, Python versions, GPUs, and WebAssembly
  - Build variants and dependencies
  - Test selection, exclusions, retries, and event restrictions
  - Artifacts and caches
  - `usd-core`, downloadable archive, and full-release processes
  - Dependencies on private Pixar systems
- Confirmation of default GPU jobs

### 2. Select runner and hosting approaches

- Resource, reliability, security, operating-effort, and cost requirements
- Options:
  - Free and paid GitHub-hosted runners
  - On-demand cloud runners
  - Purchased or colocated hardware
  - Hybrid infrastructure
  - Sponsor-provided infrastructure
- Validated workload, suite duration, storage, caching, and hardware assumptions
- Contributor-led and Linux Foundation staffing options

### 3. Obtain infrastructure and staffing approval

- AOUSD Steering Committee proposal:
  - Runner recommendation and cost model
  - Operating model and risks
  - Staffing options
- Confirmed Linux Foundation quote
- Funding and operating authority

### 4. Establish the AOUSD testing CI foundation

- Runners and supporting services
- Isolation, updates, monitoring, logging, caches, artifact retention, access
  controls, secrets, incident handling, and operating documentation
- Reusable GitHub Actions for manually supplied references
- Shared platform-independent behavior where practical
- Security and source-reference model covering reference validation, approved
  sources, isolation of untrusted code, permissions, protected environments,
  credential ownership, approvals, auditing, and separation of build and
  publication jobs

### 5. Implement manual builds and tests for initial platforms

- Linux x86-64, macOS, and Windows x86-64, WASM
- Shared behavior plus documented platform differences
- Diagnostic logs, alerting, and artifacts

### 5.5 Implement testing for initial platforms
- Additional testing phase and reporting
  

### 7. Reproduce current `usd-core` wheel builds

- Initial platforms and corresponding OpenUSD Python versions
- Validation through AOUSD testing CI

### 7.5 Establish AOUSD-controlled PyPI publication

- Confirm AOUSD PyPI organization
- Arrange `usd-core` ownership or publication access
- Credential custody, rotation, approval, auditing, and recovery
- Manual publication of validated wheel artifacts

### 7.6 Publish an AOUSD-produced `usd-core` release

- Manual build, test, approval, and publication
- Initial platforms
- Pixar coordination on ownership, version, timing, and recovery


### 8.0 Add GPU tests CI jobs

- Conditional on confirmation as default public CI
- Manual AOUSD invocation

### 9. Reconcile release tests and flaky-test handling

- Default tests aligned with release requirements
- Other tests optional by default
- Review of disabled, excluded, and event-restricted tests
- Per-test flaky treatment:
  - Defined retry and pass rule
  - Quarantine
  - Test or environment repair
- Continued improvement during later goals



### 11. Produce manually downloadable binary archives

- ZIP archives for initial platforms
- Contents, naming, metadata, retention, hosting, and publication procedure

### 12. Demonstrate a full OpenUSD release

- AOUSD-controlled workflows and infrastructure
- PyPI packages, downloadable archives, and other inventoried public steps

### 13. Obtain approval for AOUSD-managed OpenUSD releases

- Full-release demonstration and operating procedures
- Gap resolution
- Final Pixar and AOUSD approval
- Handoff, rollback, escalation, and coordination model

### 14. Assume responsibility for agreed public OpenUSD releases

- AOUSD-controlled workflows, infrastructure, and credentials
- Pixar coordination on source readiness and private validation

### 15. Add `PixarAnimationStudios/OpenUSD`-triggered AOUSD CI

- Agreed pull-request, push, and other triggers
- Status and results on originating commits or pull requests
- Protection of AOUSD credentials, networks, runners, and publication workflows
  from untrusted changes

### 16. Operate Pixar-managed and AOUSD-managed CI in parallel

- Agreed comparison period
- Coverage, correctness, reliability, duration, queue time, failure causes, and
  operating effort
- Difference investigation and AOUSD CI improvements

### 17. Switch required OpenUSD CI to AOUSD

- AOUSD results as required checks
- Production support, incident response, escalation, and rollback

### 18. Deprecate and retire the previous CI

- Deprecation announcement and transition period
- Removal of required-check and trigger responsibilities
- Preserved records and documentation
- No impact on Pixar's private validation or performance testing

### 19. Maintain and improve the production service

- Reliability, capacity, cost, security, platform coverage, test health,
  release quality, and contributor support
- Evidence-based prioritization

## Unordered beyond-one-year goals

- Tag-triggered release builds with manual publication approval
- PyPI Trusted Publishing for `usd-core`
- Linux AArch64 and WebAssembly as peer release-platform candidates
- Divided or serialized work on free or smaller GitHub-hosted runners
- Public performance testing:
  - Stable, comparable infrastructure
  - Baselines and regression policy
  - Result storage and reporting
  - Hardware-change procedures
- Public triggers and safe result reporting for Pixar's private validation and
  performance suites
- Additional release automation and platforms based on demand and capacity
- Handle CI for other AOUSD hosted repositories (Adobe's USD file format plugins when rehomed)

## Deferred decisions

- Package, archive, and full-release validation
- Required, optional, quarantined, and excluded tests
- Flaky-test pass rules
- ZIP archive contents and naming
- Repository triggers and permissions
- Parallel-CI duration and exit criteria
- Reliability, performance, cost, and service-level targets

## Public references

- [OpenUSD `BuildUSD` workflow](https://github.com/PixarAnimationStudios/OpenUSD/blob/dev/.github/workflows/buildusd.yml)
- [`usd-core` on PyPI](https://pypi.org/project/usd-core/)
- [PyPI Trusted Publishing](https://docs.pypi.org/trusted-publishers/)
