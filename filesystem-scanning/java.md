## Root dependency scanning

Trivy includes more than just root dependencies in result.  Indirect dependencies are also included in the report.

e.g.:

    pom.xml
        dep1
            dep2

for this case Trivy will include both dep1 and dep2.

-- Trivy currently does not have the option to report only on root dependencies.


See the following links on the conversation:
- https://github.com/aquasecurity/trivy/discussions/4870
- https://github.com/aquasecurity/trivy/discussions/4840


## Trivy scan shows old version of jar #4223 

I think I found something interesting. Inside the Trivy documentation, it mentions that it also look into the local cached Maven repository file location under ~/.m2/repository. If I do a full maven build under the project, it picks up the old JAR which is not used in the project nor in the target folder file system. Although we have specifically in maven project to use a newer version, maven will still download the depended version it required and stored in the local cache directory. That is why the older version is showing up in the scan report even if we specifically set a newer version in POM.XML file.

This is what you can try it out.

    Do a maven clean on the project and also truncate the ~/.m2/repository folder in that order
    Do a Trivy offline fs scan and show your initial result.
    Do a full maven clean install
    Run another Trivy offline fs scan again without truncating the ~/.m2/repository folder.
    Truncate the ~/.m2/repository folder again
    Run another Trivy offline fs scan again to see a completely different result.

4 replies
@mhou1981
mhou1981
Jun 27, 2023

Suggested change from using fs scan to use rootfs scan on maven repository after compilation. trivy rootfs scan will not scan the ~/.m2/repository.