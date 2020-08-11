# Overview
We are on the private forked version of the Kotlin gRPC SDK. This is because there is currently no way to use custom coroutine context with 0.1.4, it's coming in 0.1.5. Without that support it's hard to support opencensus, MDC and gRPC context propagation and need to resort to hacks.

# Build Locally
To build locally, run:

```
gradle :grpc-kotlin-compiler:publishToMavenLocal :grpc-kotlin-stub:publishToMavenLocal
```

# Publish to GitHub Packages
We are hosting built library at GitHub Packages.

```
gradle :grpc-kotlin-compiler:publish :grpc-kotlin-stub:publish
```

NOTE: grpc-kotlin-stub-lite depends on grpc-kotlin-stub, so you might need to re-run a build to make sure that the dependency is published to the artifactory first.

# Update gRPC SDK
## Rebase `master` branch
To rebase master first check if you have upstream set-up.

```
git remote -v

origin	git@github.com:getstep/grpc-kotlin.git (fetch)
origin	git@github.com:getstep/grpc-kotlin.git (push)
upstream	git@github.com:getstep/grpc-kotlin.git (fetch)
upstream	git@github.com:getstep/grpc-kotlin.git (push)
```

If you don't see `upstream` in the output, add one. You have to do it only once.

```
git remote add upstream git@github.com:getstep/grpc-kotlin.git
```

Now fetch upstream and rebase master.

```
git checkout master
git fetch upstream
git rebase upstream/master
git push origin master
```

Confirm in UI that the branch is even with grpc-kotlin:master.

```
This branch is even with grpc-kotlin:master.
```

## Backup `step` branch
Check current Stripe SDK version of `step` branch.

```
git checkout step
git checkout -b step-${VERSION}
```

Confirm that last commits on both `step` and `step-${VERSION}` are the same.

## Rebase `step` branch
Create staging version of rebase branch, in case things go sideways and rebase against master. Do not work on `step` backup branch.
```
git checkout step
git rebase master
```

You will need to force push to branch to the Step origin.
