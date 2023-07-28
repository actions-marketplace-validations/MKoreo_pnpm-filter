# Github Action: mkoreo/pnpm-filter@v1
This Github action sets up PNPM and makes use of the "pnpm --filter" command to determine if the source code of an app/service has changed, compared to a pull request (PR) being made.
This includes detection of changes to the monorepo packages, which are defined as dependencies of the app/service in package.json.

# Usage
Use the github action by calling the external repo github action: "mkoreo/pnpm-filter@v1"
Define input parameters:
- appName: The app or service name (foldername) to be checked for changes to its source code.
- PAT: The PAT for your github repository, to checkout the source code and pull request.

Use output parameter:
- changed: "True" if source has changed.

Example usage in workflow:
```
    steps:
    - id: pnpm-filter
      uses: mkoreo/pnpm-filter@v1
      with:
        appName: auth-service
        PAT: ${{secrets.PAT}}

    - name: Build
    if: ${{steps.pnpm-filter.outputs.changed}} == 'True'
    uses: docker/build-push-action@v4
    with:
      file: ./projects/backend/auth-service/Dockerfile
      target: test-target
      cache-from: type=gha
      cache-to: type=gha,mode=max
      context: .
```
