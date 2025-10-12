# GitHub Actions to Free Disk Space on Ubuntu runners

A customizable GitHub Actions to free disk space on Linux GitHub Actions runners.

This is a revialization of the action deveoped at [Original Project](https://github.com/jlumbroso/free-disk-space), but has not been maintained for several years.

On a typical Ubuntu runner, with all options turned on (or not turned off rather), this can clear up to 31 GB of disk space in about 3 minutes
 (the longest period is calling `apt` to uninstall packages). This is useful when you need a lot of disk space to run computations.

Please don't hesitate to [submit issues](https://github.com/BRAINSia/free-disk-space/issues)
to report problems or suggest new features (or sets of files to help remove).

Also, please ⭐️ the repo if you like this GitHub Actions! Thanks! 😊

## Example

```yaml
name: Free Disk Space (Ubuntu)
on: push

jobs:
  free-disk-space:
    runs-on: ubuntu-latest
    steps:

    - name: Free Disk Space (Ubuntu)
      uses: BRAINSia/free-disk-space@v2
      with:
        # this might remove tools that are actually needed,
        # if set to "true" but frees about 6 GB
        tool-cache: false
        
        # all of these default to true, but feel free to set to
        # "false" if necessary for your workflow
        mandb:   true
        android: true
        dotnet: true
        haskell: true
        large-packages: true
        docker-images: true
        swap-storage: true
```
## Options

Most of the options are self-explanatory.

The option `tool-cache` removes all the pre-cached tools (Node, Go, Python, Ruby, ...) that are loaded in a runner's environment, [installed in the path specified by the `AGENT_TOOLSDIRECTORY` environment variable](https://github.com/actions/virtual-environments/blob/5a2cb18a48bce5da183486b95f5494e4fd0c0640/images/linux/scripts/installers/configure-environment.sh#L25-L29) (the same environment variable is used across Windows/macOS/Linux runners, see an example of its use on [the `setup-python` GitHub Action](https://github.com/actions/setup-python)). This option was [suggested](https://github.com/actions/virtual-environments/issues/2875#issuecomment-1163392159) by [@miketimofeev](https://github.com/miketimofeev).

## Acknowledgement

This GitHub Actions came around because I kept rewriting the same few lines of `rm -rf` code.

Here are a few sources of inspiration:
- https://github.community/t/bigger-github-hosted-runners-disk-space/17267/11
- https://github.com/apache/flink/blob/master/tools/azure-pipelines/free_disk_space.sh
- https://github.com/ShubhamTatvamasi/free-disk-space-action
- https://github.com/actions/virtual-environments/issues/2875#issuecomment-1163392159
- https://github.com/easimon/maximize-build-space/
- https://github.com/jlumbroso/free-disk-space

## Typical Output

The amount of space storage saved by each option on an `ubuntu-24.04` runner is summarized here:
```
TIME_REPORT: =====================================================
TIME_REPORT:      subsection | time       seconds | size saved   |
TIME_REPORT: =====================================================
TIME_REPORT:    swap-storage | 0          seconds | 4.0GiB       |
TIME_REPORT:         haskell | 1          seconds | 6.2GiB       |
TIME_REPORT:          dotnet | 1          seconds | 3.3GiB       |
TIME_REPORT:      tool-cache | 6          seconds | 5.0GiB       |
TIME_REPORT:         android | 17         seconds | 9.6GiB       |
TIME_REPORT:  large-packages | 79         seconds | 4.6GiB       |
TIME_REPORT:           mandb | 12         seconds | 241MiB       |
TIME_REPORT:   docker-images | 0          seconds | 0B           |
TIME_REPORT: =====================================================
TIME_REPORT:            root | 139        seconds | 29GiB        |
TIME_REPORT: =====================================================
TIME_REPORT:         overall | 139        seconds | 33GiB        |
TIME_REPORT: =====================================================
```
