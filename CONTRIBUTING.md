# Contributing to Kunlun Ops

Thank you for your interest in contributing to Kunlun Ops. Kunlun Ops is a high-performance operator library and development toolkit for Baidu Kunlun XPU. We welcome feedback, bug reports, documentation improvements, test cases, and code contributions that help improve operator correctness, performance, usability, and ecosystem compatibility.

## Code of Conduct

Please keep discussions respectful, constructive, and focused on improving the project. Be clear when reporting problems, provide enough context for maintainers to reproduce issues, and assume good intent during review and discussion.

## How to Contribute

You can contribute in several ways:

- Report bugs or build issues through GitHub Issues.
- Suggest improvements for APIs, documentation, tests, or developer workflows.
- Submit fixes for operator correctness, compatibility, or performance issues.
- Add or improve test cases for existing operators.
- Improve build, profiling, or validation scripts.
- Improve documentation, examples, and troubleshooting notes.

For substantial changes, please open an issue first to discuss the motivation, expected behavior, implementation approach, and compatibility impact before submitting a pull request.

## Reporting Issues

When filing an issue, please include as much relevant information as possible:

- Kunlun Ops commit ID or release version.
- Hardware platform and XPU device information.
- Operating system, compiler, Python, PyTorch, XDNN, and related dependency versions.
- Build command or test command used.
- Complete error logs or minimal reproducible examples.
- Expected behavior and actual behavior.
- Whether the issue is a correctness, performance, build, compatibility, or documentation problem.

For performance issues, please include the operator name, input shapes, data types, benchmark command, baseline result if available, and profiling information when possible.

## Development Workflow

1. Fork the repository or create a working branch from the target branch.
2. Make your changes in a focused branch.
3. Build and test the affected modules locally.
4. Update documentation and tests when behavior changes.
5. Submit a pull request with a clear description of the change.

Please keep pull requests focused. A small, well-scoped change is easier to review and merge than a large change that mixes unrelated fixes, refactoring, formatting, and feature work.

## Build and Test

Kunlun Ops contains multiple modules. Build and test the module affected by your change before submitting a pull request.

Build optimized operators:

```bash
bash build.sh optimized_ops
```

Run optimized operator tests:

```bash
./output/optimized_ops/test/test_kunlun --gtest_filter=*.function_xpu3
```

Build XPU operator blocks:

```bash
bash build.sh xops_blocks
```

Build Kunlun Ops Python/PyTorch module:

```bash
bash build.sh kunlun_ops
```

Check the installed Kunlun Ops package:

```bash
python -m kunlun_ops --doctor
```

Run an example test:

```bash
export XPUAPI_DEBUG=0x1
export XPURT_DISPATCH_MODE=PROFILING
python3 test/easy_test_xdnn_activation.py
```

If your change requires a different setup, please describe the environment and commands used in the pull request.

## Pull Request Guidelines

A good pull request should include:

- A concise title that describes the change.
- A clear summary of what changed and why.
- Links to related issues, if any.
- Test results, benchmark results, or validation logs when relevant.
- Notes about compatibility, performance impact, or known limitations.
- Documentation updates for user-visible behavior changes.

Before submitting a pull request, please check that:

- The code builds successfully for the affected module.
- Relevant tests pass locally.
- New functionality includes tests or validation steps where practical.
- Public APIs and operator behavior are documented when needed.
- Formatting-only changes are not mixed with functional changes.
- No internal credentials, private URLs, tokens, customer data, or confidential information are included.

## Coding Guidelines

Please follow the style and structure of the surrounding code. In general:

- Keep changes minimal and focused on the problem being solved.
- Prefer clear and maintainable code over overly clever implementations.
- Avoid unrelated refactoring in feature or bug-fix pull requests.
- Use descriptive names for operators, kernels, tests, and helper functions.
- Add comments only where they clarify non-obvious logic, hardware-specific behavior, or performance-sensitive implementation details.
- Keep interfaces stable unless the change is discussed and documented.

For performance-sensitive code, please include benchmark results or profiling data when practical. If a change improves one case but may affect another, describe the trade-off in the pull request.

## Tests and Validation

Contributions that change operator behavior should include appropriate validation. Depending on the change, this may include:

- Correctness tests comparing XPU results with reference results.
- Shape, data type, boundary, and error-handling tests.
- Performance benchmarks for optimized operators.
- Regression tests for previously reported issues.
- Documentation updates for changed behavior or usage.

Please avoid adding broad or unrelated tests that make the change harder to review. Focus on the behavior directly affected by the contribution.

## Documentation

Documentation improvements are welcome. Please update documentation when your change affects:

- Build or installation steps.
- Runtime dependencies or environment variables.
- Operator APIs, supported shapes, or supported data types.
- Test commands or validation workflows.
- Known limitations or compatibility notes.

## Security and Sensitive Information

Do not report security-sensitive issues publicly if they may expose vulnerabilities, credentials, private infrastructure, customer information, or confidential implementation details. Please contact the project maintainers through the approved private channel for security-sensitive reports.

Do not include the following in issues, pull requests, commits, logs, examples, or screenshots:

- Access tokens, passwords, keys, certificates, or credentials.
- Internal-only service URLs, registry addresses, or deployment details.
- Customer data, private datasets, or confidential benchmark data.
- Proprietary third-party code or files without redistribution permission.

## License and Contribution Rights

By submitting a contribution, you confirm that you have the right to contribute the code, documentation, tests, or other materials, and that your contribution can be distributed under the project's license.

If the project requires a Contributor License Agreement (CLA), Developer Certificate of Origin (DCO), or another contribution agreement, maintainers will provide the required process before accepting the contribution.

## Review and Maintainer Response

Maintainers will review pull requests for correctness, maintainability, performance impact, compatibility, documentation, and test coverage. Review may take additional time for changes involving hardware-specific optimizations, public APIs, build systems, or external dependencies.

During review, maintainers may ask for clarifications, additional tests, benchmark data, or changes to the implementation. Please keep discussion in the pull request so that decisions and trade-offs remain visible to future contributors.

## Getting Help

If you are unsure whether a change is suitable, open an issue first and describe your use case. This helps align on the expected behavior and avoids unnecessary work before implementation.
