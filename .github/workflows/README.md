# Advanced CI Workflow Template

This repository contains an advanced CI workflow template (`ci-cache-advance.yml`) that demonstrates sophisticated caching strategies and multi-stage build processes.

## Features

### 🚀 Advanced Caching Strategy
- **Multi-level dependency caching**: Node.js, Ruby, and system dependencies
- **Build artifact caching**: Jekyll builds, test results, and security scans
- **Conditional cache restoration**: Smart cache invalidation based on file changes
- **Cross-job cache sharing**: Efficient cache reuse across matrix builds

### 🔧 Matrix Builds
- **Multi-OS support**: Ubuntu and macOS
- **Multiple runtime versions**: Node.js 16/18/20, Ruby 3.0/3.1/3.2
- **Optimized combinations**: Reduced matrix for faster execution
- **Primary build designation**: Dedicated deployment candidate

### 🛡️ Security & Quality
- **Vulnerability scanning**: Trivy security scanner integration
- **SARIF reporting**: Security findings uploaded to GitHub Security tab
- **Performance testing**: Lighthouse CI integration for web performance
- **Code quality checks**: Linting and validation steps

### 📦 Deployment Pipeline
- **Conditional deployment**: Only on main branch pushes
- **Environment protection**: Production environment with manual approval
- **Artifact management**: Build artifacts with configurable retention
- **GitHub Pages integration**: Automated deployment with custom domain support

### ⚡ Performance Optimizations
- **Parallel execution**: Matrix builds with fail-fast disabled
- **Smart caching**: Multi-level cache keys with fallback restoration
- **Conditional steps**: Skip unnecessary work based on cache hits
- **Concurrency control**: Prevent deployment conflicts

## Usage

### Basic Setup
1. Copy `.github/workflows/ci-cache-advance.yml` to your repository
2. Customize environment variables for your project
3. Update matrix configurations for your supported versions
4. Configure secrets and variables as needed

### Workflow Triggers
- **Push events**: Triggers on main and develop branches
- **Pull requests**: Full validation on PRs to main
- **Scheduled runs**: Weekly dependency freshness checks
- **Manual dispatch**: On-demand execution with cache clearing option

### Cache Management
The workflow implements a sophisticated caching strategy:

```yaml
# Cache key generation
cache-key: deps-${{ runner.os }}-node${{ env.NODE_VERSION }}-ruby${{ env.RUBY_VERSION }}-${{ hashFiles('**/package-lock.json', '**/Gemfile.lock') }}

# Fallback keys for partial cache hits
restore-keys: |
  deps-${{ runner.os }}-node${{ env.NODE_VERSION }}-ruby${{ env.RUBY_VERSION }}-
  deps-${{ runner.os }}-node${{ env.NODE_VERSION }}-
  deps-${{ runner.os }}-
```

### Environment Variables
Customize these variables for your project:

```yaml
env:
  NODE_VERSION: '18'        # Default Node.js version
  RUBY_VERSION: '3.1'       # Default Ruby version
```

### Secrets and Variables
Configure these in your repository settings:

#### Repository Secrets
- `GITHUB_TOKEN`: Automatically provided by GitHub Actions

#### Repository Variables
- `CUSTOM_DOMAIN`: (Optional) Custom domain for GitHub Pages deployment

## Customization

### Adding New Dependencies
Update the cache paths in the workflow:

```yaml
path: |
  ~/.npm                    # npm cache
  ~/.cache/yarn            # Yarn cache
  vendor/bundle            # Ruby gems
  ~/.bundle                # Bundle cache
  node_modules             # Node modules
  # Add your custom cache paths here
```

### Matrix Configuration
Modify the build matrix for your needs:

```yaml
matrix:
  os: [ubuntu-latest, macos-latest, windows-latest]  # Add Windows if needed
  node-version: ['16', '18', '20', '21']            # Update versions
  ruby-version: ['3.0', '3.1', '3.2', '3.3']       # Update versions
```

### Security Scanning
Customize Trivy scanner configuration:

```yaml
- name: Run Trivy vulnerability scanner
  uses: aquasecurity/trivy-action@master
  with:
    scan-type: 'fs'          # filesystem scan
    scan-ref: '.'
    format: 'sarif'
    output: 'trivy-results.sarif'
    severity: 'CRITICAL,HIGH'  # Filter by severity
```

## Best Practices

1. **Cache Key Design**: Include all relevant factors that affect dependencies
2. **Matrix Optimization**: Balance coverage with execution time
3. **Artifact Management**: Set appropriate retention policies
4. **Security Integration**: Regularly update scanner versions
5. **Performance Monitoring**: Track build times and cache hit rates

## Troubleshooting

### Cache Issues
- Use manual dispatch with cache clearing to reset stale caches
- Check cache key generation logic for consistency
- Verify file paths in cache configuration

### Matrix Build Failures
- Review exclude/include logic for invalid combinations
- Check environment-specific dependencies
- Verify cross-platform compatibility

### Deployment Problems
- Ensure GitHub Pages is enabled in repository settings
- Verify branch protection rules allow deployment
- Check custom domain configuration

## Contributing

When modifying this workflow:
1. Validate YAML syntax before committing
2. Test changes with workflow dispatch
3. Monitor cache hit rates and build performance
4. Document any breaking changes

---

This template provides a solid foundation for advanced CI/CD pipelines with sophisticated caching strategies. Customize it based on your specific project requirements.