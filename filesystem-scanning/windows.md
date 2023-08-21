**Question:**
When we scan host with fs command, all files are not being detected by file walk.

**Answers:**
The files analyzed vary depending on the target.
This is because Trivy primarily categorizes targets into two groups:

* Pre-build
* Post-build

If the target is a pre-build project, like a code repository, Trivy will analyze files used for building, such as lock files.
On the other hand, when the target is a post-build artifact, like a container image, Trivy will analyze installed package metadata like jar's, .gemspec, binary files, and so on.

You can see table with supported languages and modes here - https://aquasecurity.github.io/trivy/v0.44/docs/scanner/vulnerability/language/#supported-languages

In your case you need to use rootfs for jar files.

Discussion: https://github.com/aquasecurity/trivy/discussions/4998
