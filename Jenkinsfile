#!/usr/bin/groovy

@Library("GetchaJenLib") _

// <Globals>
// ---------------------------------------------------------------------------------------------------------------------
// stages that can be skipped if authorized
globals.set(
  "SKIPPABLE_STAGES",
  [
    "Checkout Code",
    "Validate Artifacts",
    "Secrets Scan",
    "Get Current Version",
    "Calculate Version",
    "Update Version",
    "Generate Changelog",
    "Build",
    "Test",
    "Lint",
    "OWASP Dependency Check",
    "SonarQube Scan",
    "Veracode Scan",
    "Version/Changelog Push",
    "Build Artifacts Push",
    "App Deployment"
  ]
)

// The list of user emails that can skip a stage via a merge request comment. If a merge request fails, a user can enter
// a special comment in GitLab to make Jenkins retry the job. The comments applicable to different users are below.
//
// Regular User:                          Jenkins Retry, Skip Phases - none
// Stage Skip User (those on below list): Jenkins Retry, Skip Phases - stage name,stage name, stage name
//
// The names of the stages must be separated by commas.
globals.set("STAGE_SKIP_USER_EMAILS", [ "john.smith@example.com", "jane.smith@example.com" ])

// the list of user emails that can deploy to test
globals.set("TEST_DEPLOYERS_USER_EMAILS", [ "john.smith@example.com", "jane.smith@example.com" ])

// the list of user emails that can deploy to production
globals.set("PRODUCTION_DEPLOYERS_USER_EMAILS", [ "john.smith@example.com", "jane.smith@example.com" ])

// the credentials ID of the accepted token that GitLab sends with the webhook
globals.set("WEBHOOK_TOKEN_CRED_ID", "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee")

// the name of the branch to use when manually invoking the job in Jenkins rather than an outside trigger
if (env.chosenBranch) {
  globals.set("MANUAL_JOB_INVOCATION_BRANCH", env.chosenBranch.toString())
} else {
  globals.set("MANUAL_JOB_INVOCATION_BRANCH", "")
}

// valid branches that this job can use
globals.set("VALID_BRANCHES", [ "latest", "test", "master" ])

// the development, test, and production branch regular expressions as Pattern instances
globals.set("DEVELOPMENT_BRANCH_REGEX", ~/^latest$/)
globals.set("TEST_BRANCH_REGEX", ~/^test$/)
globals.set("PRODUCTION_BRANCH_REGEX", ~/^master$/)

// the note regex string which will trigger a job in merge requests
globals.set("MR_NOTE_REGEX_STRING", "Jenkins Retry, Skip Stages - .+")

// the branches that can trigger a build
globals.set("BUILD_TRIGGER_BRANCHES_CSV", "latest,test,master")

// the branches which are excluded from ci-skip on push
globals.set("CI_SKIP_ON_PUSH_EXCL_BRANCHES", "master")

// the max number of builds to keep (i.e. the max number of job history entries)
globals.set("MAX_BUILDS_TO_KEEP", "25")

// the total time the job instance can run before it times out
globals.set("JOB_INSTANCE_TIMEOUT_MINS", 180)

// the max amount of times to retry the specified stage
globals.set("CHECKOUT_CODE_STAGE_RETRY_COUNT", 2)
globals.set("VALIDATE_ARTIFACTS_STAGE_RETRY_COUNT", 2)
globals.set("SECRETS_SCAN_STAGE_RETRY_COUNT", 0)
globals.set("CONFIG_VALIDATION_STAGE_RETRY_COUNT", 0)
globals.set("GET_CURRENT_VERSION_STAGE_RETRY_COUNT", 0)
globals.set("CALC_VERSION_STAGE_RETRY_COUNT", 0)
globals.set("UPDATE_VERSION_STAGE_RETRY_COUNT", 0)
globals.set("CHANGELOG_GEN_STAGE_RETRY_COUNT", 0)
globals.set("BUILD_STAGE_RETRY_COUNT", 0)
globals.set("TEST_STAGE_RETRY_COUNT", 0)
globals.set("LINT_STAGE_RETRY_COUNT", 0)
globals.set("OWASP_DEP_CHECK_STAGE_RETRY_COUNT", 0)
globals.set("SONARQUBE_SCANNING_STAGE_RETRY_COUNT", 1)
globals.set("VERACODE_SCANNING_STAGE_RETRY_COUNT", 1)
globals.set("STAGE_COMMIT_FILES_STAGE_RETRY_COUNT", 0)
globals.set("VERSION_CHANGELOG_PUSH_STAGE_RETRY_COUNT", 0)
globals.set("PUSH_BUILD_ARTIFACTS_STAGE_RETRY_COUNT", 2)
globals.set("APP_DEPLOYMENT_STAGE_RETRY_COUNT", 0)

// the name of the application related to this job
globals.set("APPLICATION_NAME", "Spring Boot Demo")

// the name of the Docker image for the application this Jenkins job belongs to
globals.set("DOCKER_IMAGE_NAME", "docker-registry.internal.example.com/test/spring-boot-demo")

// the private Docker registry FQDN and port
globals.set("DOCKER_PRIVATE_REGISTRY_FQDN_AND_PORT", "docker-registry.internal.example.com")

// the private Docker registry URL where images are pulled from and pushed to
globals.set("DOCKER_PRIVATE_REGISTRY_URL", "https://${globals.get('DOCKER_PRIVATE_REGISTRY_FQDN_AND_PORT')}")

// the credential ID needed to access the private Docker registry
globals.set("DOCKER_PRIVATE_REGISTRY_CRED_ID", "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee")

// the public Docker registry FQDN and port
globals.set("DOCKER_PUBLIC_REGISTRY_FQDN_AND_PORT", "docker-hub.internal.example.com")

// the public Docker registry URL where images are pulled from and pushed to
globals.set("DOCKER_PUBLIC_REGISTRY_URL", "https://${globals.get('DOCKER_PUBLIC_REGISTRY_FQDN_AND_PORT')}")

// the credential ID needed to access the public Docker registry
globals.set("DOCKER_PUBLIC_REGISTRY_CRED_ID", "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee")

// if set to true, the Docker image will be built using the available cache
// if set to false, the Docker image won't use cache and will rebuild every layer
globals.set("USE_IMAGE_CACHE", true)

// the resource limits for Docker containers spawned by this job instance
globals.set("DOCKER_CONTAINER_CPU_CAP", "2")
globals.set("DOCKER_CONTAINER_MEMORY_CAP", "2048m")

// the name of the network to use for Docker containers spawned by this job instance
globals.set("DOCKER_CONTAINER_NETWORK", "cicd-tools")

// the persistent volume base path on the host used by the CICD container tools to persist data
globals.set("PERSISTENT_VOL_HOST_BASE_PATH", "/network-data/persistent-volumes/docker/cicd")

// the parent workspace host path used by Jenkins
globals.set("JENKINS_PARENT_WORKSPACE_HOST_PATH",
  "${globals.get('PERSISTENT_VOL_HOST_BASE_PATH')}/jenkins/home/workspace/${JOB_NAME}")

// the parent workspace container path used by Jenkins
globals.set("JENKINS_PARENT_WORKSPACE_CONTAINER_PATH", "/var/jenkins_home/workspace/${JOB_NAME}")

// the name of the folder that stores all the job instance data used by Jenkins
globals.set("JENKINS_JOB_INSTANCE_COLLECTION_FOLDER", "builds")

// the child workspace host path for the current job instance used by Jenkins
globals.set("JENKINS_CHILD_WORKSPACE_HOST_PATH",
  "${globals.get('JENKINS_PARENT_WORKSPACE_HOST_PATH')}" +
  "/${globals.get('JENKINS_JOB_INSTANCE_COLLECTION_FOLDER')}/${BUILD_NUMBER}")

// the child workspace container path for the current job instance
globals.set("JENKINS_CHILD_WORKSPACE_CONTAINER_PATH",
  "${globals.get('JENKINS_PARENT_WORKSPACE_CONTAINER_PATH')}" +
  "/${globals.get('JENKINS_JOB_INSTANCE_COLLECTION_FOLDER')}/${BUILD_NUMBER}")

// the name of the merge request locked branches Jenkins container path and file name
globals.set("MR_LOCKED_BRANCHES_JENKINS_CONTAINER_PATH",
  "${globals.get('JENKINS_PARENT_WORKSPACE_CONTAINER_PATH')}/merge-req-data")
globals.set("MR_LOCKED_BRANCHES_FILE_NAME", "merge-request-locked-branches.txt")

// the internal root URL of the SonarQube server, including the port (port can be removed if using 443 or 80)
globals.set("SONARQUBE_INTERNAL_ROOT_URL", "http://sonarqube:9000")

// the external root URL of the SonarQube server, including the port (port can be removed if using 443 or 80)
globals.set("SONARQUBE_EXTERNAL_ROOT_URL", "https://sonarqube.internal.example.com")

// the project ID as shown in SonarQube
globals.set("SONARQUBE_PROJECT_ID", "test_spring-boot-demo")

// the internal URL for this project as shown on SonarQube
globals.set("SONARQUBE_INTERNAL_PROJECT_URL",
  "${globals.get('SONARQUBE_INTERNAL_ROOT_URL')}/dashboard?id=${globals.get('SONARQUBE_PROJECT_ID')}")

// the external URL for this project as shown on SonarQube
globals.set("SONARQUBE_EXTERNAL_PROJECT_URL",
  "${globals.get('SONARQUBE_EXTERNAL_ROOT_URL')}/dashboard?id=${globals.get('SONARQUBE_PROJECT_ID')}")

// the time in seconds to wait between each SonarQube query to get updated information on scanned code
globals.set("SONARQUBE_QUERY_INTERVAL", 10)

// the credential ID needed to access the SonarQube API
globals.set("SONARQUBE_CRED_ID", "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee")

// the credentials ID for Veracode scanning
globals.set("VERACODE_CRED_ID", "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee")

// the scan options for Veracode - see https://help.veracode.com/go/t_install_jenkins for more details
globals.set("VERACODE_SCAN_OPTIONS", [
  "applicationName": globals.get("APPLICATION_NAME"),
  "canFailJob": "true",
  "criticality": "VeryLow", // values include VeryHigh, High, Medium, Low, VeryLow
  "debug": "true",
  "sandboxName": "",
  "scanName": "build-" + env.BUILD_NUMBER.toString(),
  "teams": "Example Team",
  "timeout": "60",
  "uploadIncludesPattern": "**/**.jar",
  "waitForScan": "true"
])

// email recipients (separated by commas) where job notifications should be sent
globals.set("EMAIL_NOTIFICATION_RECIPIENTS", "john.smith@example.com,jane.smith@example.com")

// the email addresses that populate when replying to the email notification
globals.set("EMAIL_NOTIFICATION_REPLY_TO_ADDRESSES", "john.smith@example.com,jane.smith@example.com")

// the file attachment pattern to include files as attachments in email notifications
// - see https://github.com/jenkinsci/email-ext-plugin#attachments for more details
globals.set("EMAIL_FILE_ATTACHMENT_PATTERN", "")

// name and email address used by Git and notification emails
globals.set("CICD_ADMIN_NAME", "Jenkins Administrator")
globals.set("CICD_ADMIN_EMAIL", "jenkins-no-reply@example.com")

// the repo URLs used by Maven (overwrites anything defined in pom.xml)
globals.set("MAVEN_INTERNAL_RELEASE_REPO_URL", "http://nexus:8081/repository/maven-releases")
globals.set("MAVEN_INTERNAL_SNAPSHOT_REPO_URL", "http://nexus:8081/repository/maven-snapshots")

// name of the image and tag to use for performing Maven operations
globals.set("MAVEN_IMAGE_NAME", "docker-registry.internal.example.com/cicd/maven")
globals.set("MAVEN_IMAGE_TAG", "3.6.3-amazoncorretto-11")

// path to Maven workspace on the host
globals.set("MAVEN_WORKSPACE_HOST_PATH", globals.get("JENKINS_CHILD_WORKSPACE_HOST_PATH"))

// path to Maven workspace on the Maven container
globals.set("MAVEN_WORKSPACE_CONTAINER_PATH", "/usr/src/maven-project")

// path to Maven config on the Jenkins container
globals.set("MAVEN_CONFIG_JENKINS_CONTAINER_PATH",
  "${globals.get('JENKINS_PARENT_WORKSPACE_CONTAINER_PATH')}/tools-data/maven/config")

// path to Maven settings file on the host
globals.set("MAVEN_SETTINGS_FILE_HOST_PATH",
  "${globals.get('PERSISTENT_VOL_HOST_BASE_PATH')}/maven/config/settings.xml")

// path to Maven settings file on the Maven container
globals.set("MAVEN_SETTINGS_FILE_CONTAINER_PATH", "/usr/share/maven/ref/settings.xml")

// path to Maven settings file on the host
globals.set("MAVEN_SEC_SETTINGS_FILE_HOST_PATH",
  "${globals.get('PERSISTENT_VOL_HOST_BASE_PATH')}/maven/config/settings-security.xml")

// path to Maven settings file on the Maven container
globals.set("MAVEN_SEC_SETTINGS_FILE_CONTAINER_PATH", "/usr/share/maven/ref/settings-security.xml")

// path to Maven local repo on the Jenkins container
globals.set("MAVEN_LOCAL_REPO_JENKINS_CONTAINER_PATH",
  "${globals.get('JENKINS_PARENT_WORKSPACE_CONTAINER_PATH')}/tools-data/maven/local-repo")

// path to local Maven repo on the host
globals.set("MAVEN_LOCAL_REPO_HOST_PATH",
  "${globals.get('JENKINS_PARENT_WORKSPACE_HOST_PATH')}/tools-data/maven/local-repo")

// path to local Maven repo on the Maven container
globals.set("MAVEN_LOCAL_REPO_CONTAINER_PATH", "/var/maven/.m2/repositories")

// path to Maven local SonarQube data on the Jenkins container
globals.set("MAVEN_LOCAL_SONARQUBE_DATA_JENKINS_CONTAINER_PATH",
  "${globals.get('JENKINS_PARENT_WORKSPACE_CONTAINER_PATH')}/tools-data/maven/sonarqube")

// path to local SonarQube data on the host
globals.set("MAVEN_LOCAL_SONARQUBE_DATA_HOST_PATH",
  "${globals.get('JENKINS_PARENT_WORKSPACE_HOST_PATH')}/tools-data/maven/sonarqube")

// path to local SonarQube data on the Maven container
globals.set("MAVEN_LOCAL_SONARQUBE_DATA_CONTAINER_PATH", "/var/maven/.sonar")

// volume mappings used by Docker when performing Maven operations
globals.set("MAVEN_VOLUME_OPTS",
  "-v ${globals.get('MAVEN_WORKSPACE_HOST_PATH')}:${globals.get('MAVEN_WORKSPACE_CONTAINER_PATH')} " +
  "-v ${globals.get('MAVEN_SETTINGS_FILE_HOST_PATH')}:${globals.get('MAVEN_SETTINGS_FILE_CONTAINER_PATH')}:ro " +
  "-v ${globals.get('MAVEN_SEC_SETTINGS_FILE_HOST_PATH')}:${globals.get('MAVEN_SEC_SETTINGS_FILE_CONTAINER_PATH')}:ro " +
  "-v ${globals.get('MAVEN_LOCAL_REPO_HOST_PATH')}:${globals.get('MAVEN_LOCAL_REPO_CONTAINER_PATH')} " +
  "-v ${globals.get('MAVEN_LOCAL_SONARQUBE_DATA_HOST_PATH')}:${globals.get('MAVEN_LOCAL_SONARQUBE_DATA_CONTAINER_PATH')}")

// environment variables used by Docker when performing Maven operations
globals.set("MAVEN_ENV_OPTS", "-e MAVEN_JOB_NAME=${JOB_NAME} -e MAVEN_OPTS=-Xmx512m")

// the combined Maven container options (uses the same network where Nexus is running for proper name resolution for
// items in the settings.xml file)
globals.set("MAVEN_CONTAINER_OPTS",
  "--cpus ${globals.get('DOCKER_CONTAINER_CPU_CAP')} --memory ${globals.get('DOCKER_CONTAINER_MEMORY_CAP')} " +
  "--network ${globals.get('DOCKER_CONTAINER_NETWORK')} ${globals.get('MAVEN_VOLUME_OPTS')} ${globals.get('MAVEN_ENV_OPTS')} " +
  "--entrypoint=''")

// name of the image and tag to use for performing Dockerfile linting
// see https://hub.docker.com/r/hadolint/hadolint
globals.set("HADOLINT_IMAGE_NAME", "docker-hub.internal.example.com/hadolint/hadolint")
globals.set("HADOLINT_IMAGE_TAG", "v1.18.0-6-ga0d655d-alpine")

// the directory where the Dockerfile to lint is located on the host
globals.set("HADOLINT_WORKSPACE_HOST_PATH", globals.get("JENKINS_CHILD_WORKSPACE_HOST_PATH"))

// the directory where the Dockerfile to lint is located on the container
globals.set("HADOLINT_WORKSPACE_CONTAINER_PATH", "/opt/hadolint/workspace")

// volume mappings used by Docker when performing Dockerfile linting operations
globals.set("HADOLINT_VOLUME_OPTS",
  "-v ${globals.get('HADOLINT_WORKSPACE_HOST_PATH')}:${globals.get('HADOLINT_WORKSPACE_CONTAINER_PATH')}")

// the combined Hadolint container options
globals.set("HADOLINT_CONTAINER_OPTS",
  "--cpus ${globals.get('DOCKER_CONTAINER_CPU_CAP')} --memory ${globals.get('DOCKER_CONTAINER_MEMORY_CAP')} " +
  "--network ${globals.get('DOCKER_CONTAINER_NETWORK')} ${globals.get('HADOLINT_VOLUME_OPTS')}")

// the name of the configuration file used by Hadolint
globals.set("HADOLINT_CONFIG_FILE_NAME", ".hadolint.yaml")

// the Docker trusted registries which are acceptable to use - must be listed first in Hadolint config file
globals.set("DOCKER_TRUSTED_REGISTRIES", [
  globals.get("DOCKER_PRIVATE_REGISTRY_FQDN_AND_PORT"),
  globals.get("DOCKER_PUBLIC_REGISTRY_FQDN_AND_PORT")
])

// rules to ignore when linting Dockerfiles - see https://github.com/hadolint/hadolint#rules
// - must be listed second in Hadolint config file
globals.set("DOCKERFILE_LINTER_IGNORED_RULES", [
  "DL4006"
])

// name of the image and tag to use for performing application versioning and changelog generation
// see https://github.com/GetchaDEAGLE/eagle-versioner
globals.set("EAGLE_VERSIONER_IMAGE_NAME", "docker-registry.internal.example.com/cicd/eagle-versioner")
globals.set("EAGLE_VERSIONER_IMAGE_TAG", "1.3.2")

// path to Git repo on the host running Eagle Versioner
globals.set("EAGLE_VERSIONER_WORKSPACE_HOST_PATH", globals.get("JENKINS_CHILD_WORKSPACE_HOST_PATH"))

// path to Git repo on the Eagle Versioner container
globals.set("EAGLE_VERSIONER_WORKSPACE_CONTAINER_PATH", "/git-repo")

// volume mappings used by Docker when performing application versioning
globals.set("EAGLE_VERSIONER_VOLUME_OPTS",
  "-v ${globals.get('EAGLE_VERSIONER_WORKSPACE_HOST_PATH')}:${globals.get('EAGLE_VERSIONER_WORKSPACE_CONTAINER_PATH')}")

// the combined Eagle Versioner container options
globals.set("EAGLE_VERSIONER_CONTAINER_OPTS",
  "--cpus ${globals.get('DOCKER_CONTAINER_CPU_CAP')} --memory ${globals.get('DOCKER_CONTAINER_MEMORY_CAP')} " +
  "--network ${globals.get('DOCKER_CONTAINER_NETWORK')} ${globals.get('EAGLE_VERSIONER_VOLUME_OPTS')}")

// name of the image and tag to use for secrets scanning
globals.set("GITLEAKS_IMAGE_NAME", "docker-hub.internal.example.com/zricethezav/gitleaks")
globals.set("GITLEAKS_IMAGE_TAG", "v6.1.2")

// path to Git repo on the host running GitLeaks
globals.set("GITLEAKS_WORKSPACE_HOST_PATH", globals.get("JENKINS_CHILD_WORKSPACE_HOST_PATH"))

// path to Git repo on the GitLeaks container
globals.set("GITLEAKS_WORKSPACE_CONTAINER_PATH", "/git-repo")

// volume mappings used by Docker when performing secrets scanning
globals.set("GITLEAKS_VOLUME_OPTS",
  "-v ${globals.get('GITLEAKS_WORKSPACE_HOST_PATH')}:${globals.get('GITLEAKS_WORKSPACE_CONTAINER_PATH')}")

// the combined GitLeaks container options
globals.set("GITLEAKS_CONTAINER_OPTS",
  "--cpus ${globals.get('DOCKER_CONTAINER_CPU_CAP')} --memory ${globals.get('DOCKER_CONTAINER_MEMORY_CAP')} " +
  "--network ${globals.get('DOCKER_CONTAINER_NETWORK')} ${globals.get('GITLEAKS_VOLUME_OPTS')} --entrypoint \"\"")

// the name of the file containing the GitLeaks configuration
// (for examples, see https://github.com/zricethezav/gitleaks/tree/master/examples)
globals.set("GITLEAKS_CONFIG_FILE_NAME", ".gitleaks.toml")

// name of the image and tag to use for performing OWASP Dependency Check operations
// see https://hub.docker.com/r/owasp/dependency-check
globals.set("OWASP_DEP_CHECK_IMAGE_NAME", "docker-hub.internal.example.com/owasp/dependency-check")
globals.set("OWASP_DEP_CHECK_IMAGE_TAG", "6.0.1")

// path to OWASP Dependency Check workspace on the host
globals.set("OWASP_DEP_CHECK_WORKSPACE_HOST_PATH", globals.get("JENKINS_CHILD_WORKSPACE_HOST_PATH"))

// path to OWASP Dependency Check workspace on the container
globals.set("OWASP_DEP_CHECK_WORKSPACE_CONTAINER_PATH", "/src")

// path to OWASP Dependency CHeck data on the Jenkins container
globals.set("OWASP_DEP_CHECK_DATA_JENKINS_CONTAINER_PATH",
  "${globals.get('JENKINS_PARENT_WORKSPACE_CONTAINER_PATH')}/tools-data/owasp-dep-check/data")

// the OWASP Dependency Check data host path
globals.set("OWASP_DEP_CHECK_DATA_HOST_PATH",
  "${globals.get('JENKINS_PARENT_WORKSPACE_HOST_PATH')}/tools-data/owasp-dep-check/data")

// the OWASP Dependency Check data container path
globals.set("OWASP_DEP_CHECK_DATA_CONTAINER_PATH", "/usr/share/dependency-check/data")

// volume mappings used by Docker when performing OWASP Dependency Check operations
globals.set("OWASP_DEP_CHECK_VOLUME_OPTS",
  "-v ${globals.get('OWASP_DEP_CHECK_WORKSPACE_HOST_PATH')}:${globals.get('OWASP_DEP_CHECK_WORKSPACE_CONTAINER_PATH')} " +
  "-v ${globals.get('OWASP_DEP_CHECK_DATA_HOST_PATH')}:${globals.get('OWASP_DEP_CHECK_DATA_CONTAINER_PATH')}")

// the combined OWASP Dependency Check container options
globals.set("OWASP_DEP_CHECK_CONTAINER_OPTS",
  "--cpus ${globals.get('DOCKER_CONTAINER_CPU_CAP')} --memory ${globals.get('DOCKER_CONTAINER_MEMORY_CAP')} " +
  "--network ${globals.get('DOCKER_CONTAINER_NETWORK')} ${globals.get('OWASP_DEP_CHECK_VOLUME_OPTS')} " +
  "--entrypoint=''")

// will cause job to fail if any vulnerabilities detected are greater than or equal to the score number between 0 and 10,
// setting this to 0 will never cause a job failure
// 0       = None
// 1 to 3  = Low
// 4 to 6  = Medium
// 7 to 8  = High
// 9 to 10 = Critical
// see https://jeremylong.github.io/DependencyCheck/dependency-check-cli/arguments.html
globals.set("OWASP_DEP_CHECK_FAIL_SCORE", "7")

// the name of the file containing the OWASP Dependency Check suppressions (allows for ignoring potential false positives)
// see https://jeremylong.github.io/DependencyCheck/general/suppression.html
globals.set("OWASP_DEP_CHECK_SUPPRESSION_FILE_NAME", "owasp-dep-check-suppressions.xml")

// the Docker image run command for scanning the Docker image for vulnerabilities using Trivy - this is used
// to ensure it isn't missing from the Dockerfile (for more info, see https://github.com/aquasecurity/trivy)
globals.set("TRIVY_DOCKER_IMAGE_RUN_COMMAND",
  "RUN set -euf \\\n  && curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/master/contrib/install.sh " +
  "| sh -s -- -b /usr/local/bin \\\n  && trivy filesystem --exit-code 1 --severity HIGH,CRITICAL --no-progress /")

// the name of the GitLab connection configured under Manage Jenkins -> Configure System
globals.set("GITLAB_CONNECTION_NAME", "GitLab")

// Credentials for accessing the repository to perform Git operations (e.g. pull, push, etc.). These credentials
// are created in Jenkins and the resulting ID obtained and placed here.
globals.set("GITLAB_USER_CRED_ID", "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee")

// The GitLab API credentials ID. This allows for this job instance to access the GitLab API to download raw files,
// get project details, etc. (Note: this is different than the GitLab API credential type used by the GitLab plugin
// and instead uses username/password as the credential type since the former isn't compatible with the withCredentials
// step).
globals.set("GITLAB_API_CURL_CRED_ID", "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee")

// The GitLab API credentials ID used by the GitLab plugin. This uses the GitLab API credential type instead of the
// username/password credential type (see above for more details).
globals.set("GITLAB_API_CRED_ID", "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee")

// The GitLab project groups (including subgroups). A forward slash should separate each group (e.g. group/subgroup).
// Leave this as an empty string if no groups are being used.
globals.set("GITLAB_PROJECT_GROUPS", "test")

// the name of the Git project belonging to this Jenkins job
globals.set("GITLAB_PROJECT_NAME", "spring-boot-demo")

// the internal root URL of the GitLab server, including the port (port can be removed if using 443)
globals.set("GITLAB_INTERNAL_ROOT_URL", "http://gitlab")

// The ID of the repository for this project. In order to get the correct ID, view repo in GitLab or
// visit <GITLAB_INTERNAL_ROOT_URL>/api/v4/projects. See https://docs.gitlab.com/ee/api/repository_files.html for more details.
globals.set("GITLAB_REPO_PROJECT_ID", 1)

// the URL to receive merge request information for this project using the GitLab API
globals.set("GITLAB_MERGE_REQ_INTERNAL_PATH_URL",
  "${globals.get('GITLAB_INTERNAL_ROOT_URL')}/api/v4/projects/${globals.get('GITLAB_REPO_PROJECT_ID')}/merge_requests")

// The ID of the configuration repository for this project. In order to get the correct ID, view repo in GitLab or
// visit <GITLAB_INTERNAL_ROOT_URL>/api/v4/projects. See https://docs.gitlab.com/ee/api/repository_files.html for more details.
globals.set("GITLAB_CONFIG_REPO_PROJECT_ID", 2)

// the URL of the application's configuration files which can be used to download raw files through the GitLab API
globals.set("GITLAB_CONFIG_FILES_INTERNAL_PATH_URL",
  "${globals.get('GITLAB_INTERNAL_ROOT_URL')}/api/v4/projects/${globals.get('GITLAB_CONFIG_REPO_PROJECT_ID')}/repository/files")

// the sub-path containing Git config files
globals.set("GITLAB_GIT_CONFIG_FILES_SUB_PATH", "/git")

// the Git reference used when downloading files using a supported REST API
globals.set("GIT_FILE_DOWNLOAD_REF", "master")

// the internal Git upstream repository URL used for checkouts in the context of the Jenkins worker
globals.set("GIT_UPSTREAM_INTERNAL_REPO_URL",
  "${globals.get('GITLAB_INTERNAL_ROOT_URL')}/${globals.get('GITLAB_PROJECT_GROUPS')}/${globals.get('GITLAB_PROJECT_NAME')}.git")

// properly set the forked repo if the GitLab action exists, otherwise it should be empty
if (env.gitlabActionType) {
  globals.set("GIT_FORKED_INTERNAL_REPO_URL",
    "${globals.get('GITLAB_INTERNAL_ROOT_URL')}" +
    "/${env.gitlabSourceRepoSshUrl.toString().substring(env.gitlabSourceRepoSshUrl.toString().indexOf(':') + 1)}")
} else {
  globals.set("GIT_FORKED_INTERNAL_REPO_URL", "N/A")
}
// ---------------------------------------------------------------------------------------------------------------------
// </Globals>

// <Methods>
// ---------------------------------------------------------------------------------------------------------------------
// Note: The below methods call the logic that would normally exist in a stage. Due to the Method Code Too Large error,
//       these methods had to be created here outside the node block. Please see
//       https://support.cloudbees.com/hc/en-us/articles/360039361371-Method-Code-Too-Large-Error for more details.
// ---------------------------------------------------------------------------------------------------------------------

/**
* Invokes the Prepare and Enforce Stage Logic.
*/
void callPrepareAndEnforceLogic() {
  if (env.CLOUD_HOST_IP && env.CLOUD_HOST_CREDS_ID) {
    echo "Using Docker host with IP ${CLOUD_HOST_IP} and credentials ID ${CLOUD_HOST_CREDS_ID} to process " +
      "the current Jenkins job instance."
  } else {
    error("Missing the CLOUD_HOST_IP and CLOUD_HOST_CREDS_ID environment variables. Please ensure these " +
      "are specified in the Jenkins cloud configuration and try again.")
  }

  // create necessary directories
  jobDataManager.createDirectories([
    globals.get("JENKINS_CHILD_WORKSPACE_CONTAINER_PATH"),
    globals.get("MAVEN_CONFIG_JENKINS_CONTAINER_PATH"),
    globals.get("MAVEN_LOCAL_REPO_JENKINS_CONTAINER_PATH"),
    globals.get("MAVEN_LOCAL_SONARQUBE_DATA_JENKINS_CONTAINER_PATH"),
    globals.get("OWASP_DEP_CHECK_DATA_JENKINS_CONTAINER_PATH"),
    globals.get("MR_LOCKED_BRANCHES_JENKINS_CONTAINER_PATH")
  ])

  // print important GitLab environment details (useful for debugging)
  infoPeeker.printGitlabEnvDetails()

  // set requested stages to skip
  if ((env.gitlabActionType && (env.gitlabActionType.toString() == "MERGE" || env.gitlabActionType.toString() == "NOTE"))
      && env.gitlabSourceBranch && env.gitlabTargetBranch
      && (gitRunner.isValidRef(env.gitlabSourceBranch.toString(), globals.get("PRODUCTION_BRANCH_REGEX"))
      && gitRunner.isValidRef(env.gitlabTargetBranch.toString(), globals.get("DEVELOPMENT_BRANCH_REGEX")))
      || (gitRunner.isValidRef(env.gitlabSourceBranch.toString(), globals.get("DEVELOPMENT_BRANCH_REGEX"))
      && gitRunner.isValidRef(env.gitlabTargetBranch.toString(), globals.get("TEST_BRANCH_REGEX")))) {
    globals.set("STAGES_TO_SKIP", globals.get("SKIPPABLE_STAGES"))
  } else if (env.gitlabActionType && env.gitlabActionType.toString() == "NOTE" && env.gitlabUserEmail
      && globals.get("STAGE_SKIP_USER_EMAILS").contains(env.gitlabUserEmail.toString()) && env.gitlabTriggerPhrase) {
    globals.set("STAGES_TO_SKIP",
      jobDataManager.getStagesToSkip(globals.get("SKIPPABLE_STAGES"), env.gitlabTriggerPhrase.toString()))
  } else if (env.stagesToSkip) {
    globals.set("STAGES_TO_SKIP", jobDataManager.getStagesToSkip(globals.get("SKIPPABLE_STAGES"), env.stagesToSkip.toString()))
  } else {
    globals.set("STAGES_TO_SKIP", [])
  }

  if (env.gitlabActionType && env.gitlabSourceBranch && env.gitlabTargetBranch && env.gitlabUserEmail
      && (env.gitlabActionType.toString() == "MERGE" || env.gitlabActionType.toString() == "NOTE")) {
    if (merReqAuthorizer.isAllowed(globals.get("MR_LOCKED_BRANCHES_JENKINS_CONTAINER_PATH"),
        globals.get("MR_LOCKED_BRANCHES_FILE_NAME"), env.gitlabTargetBranch.toString())) {
      echo "The target branch for this merge request isn't locked. Proceeding..."
    } else if (gitRunner.isValidRef(env.gitlabSourceBranch.toString(), globals.get("PRODUCTION_BRANCH_REGEX"))
        && gitRunner.isValidRef(env.gitlabTargetBranch.toString(), globals.get("DEVELOPMENT_BRANCH_REGEX"))) {
      echo "Since the source branch of this merge request is for production and the target branch is for development, " +
        "proceeding with job instance even though the target branch is locked."
    } else {
      error("The target branch for this merge request is locked. Please wait for it to become unlocked and try again.")
    }

    if (gitRunner.isValidRef(env.gitlabTargetBranch.toString(), globals.get("PRODUCTION_BRANCH_REGEX"))) {
      if (globals.get("PRODUCTION_DEPLOYERS_USER_EMAILS").contains(env.gitlabUserEmail.toString())) {
        echo "The user with email ${env.gitlabUserEmail.toString()} is allowed to promote to production."
      } else {
        error("The user with email ${env.gitlabUserEmail.toString()} isn't allowed to promote to production.")
      }

      if (gitRunner.isValidRef(env.gitlabSourceBranch.toString(), globals.get("DEVELOPMENT_BRANCH_REGEX"))) {
        echo "Promoting to production is allowed because the development source branch is being used."
      } else {
        error("Promoting to production is only possible if using the development source branch.")
      }
    } else if (gitRunner.isValidRef(env.gitlabTargetBranch.toString(), globals.get("TEST_BRANCH_REGEX"))) {
      if (globals.get("TEST_DEPLOYERS_USER_EMAILS").contains(env.gitlabUserEmail.toString())) {
        echo "The user with email ${env.gitlabUserEmail.toString()} is allowed to promote to test."
      } else {
        error("The user with email ${env.gitlabUserEmail.toString()} isn't allowed to promote to test.")
      }

      if (gitRunner.isValidRef(env.gitlabSourceBranch.toString(), globals.get("DEVELOPMENT_BRANCH_REGEX"))) {
        echo "Promoting to test is allowed because the development source branch is being used."
      } else {
        error("Promoting to test is only possible if using the development source branch.")
      }
    }

    // if the merge request target branch is production or the source branch is production and the target branch is
    // development, lock all branches so additional merge requests aren't possible
    if (gitRunner.isValidRef(env.gitlabTargetBranch.toString(), globals.get("PRODUCTION_BRANCH_REGEX"))
        || (gitRunner.isValidRef(gitlabSourceBranch, globals.get("PRODUCTION_BRANCH_REGEX"))
        && gitRunner.isValidRef(env.gitlabTargetBranch.toString(), globals.get("DEVELOPMENT_BRANCH_REGEX")))) {
      merReqAuthorizer.lockTargetBranches(globals.get("MR_LOCKED_BRANCHES_JENKINS_CONTAINER_PATH"),
        globals.get("MR_LOCKED_BRANCHES_FILE_NAME"), globals.get("VALID_BRANCHES"))
    } else {
      echo "Skipped locking target branches for merge request since this job instance isn't for promotion to production."
    }

    List mergeReqCommitMsgs =
      gitRunner.getMergeReqCommitMsgs(
        globals.get("GITLAB_INTERNAL_ROOT_URL"),
        [
          "gitServerCredentialsId" : globals.get("GITLAB_USER_CRED_ID"),
          "gitServerApiTokenCredId": globals.get("GITLAB_API_CURL_CRED_ID")
        ],
        [
          "userFullName"    : globals.get("CICD_ADMIN_NAME"),
          "userEmailAddress": globals.get("CICD_ADMIN_EMAIL")
        ],
        globals.get("GITLAB_MERGE_REQ_INTERNAL_PATH_URL"),
        env.gitlabMergeRequestId.toString()
      )

    // validate all commit messages that are a part of the merge request and ensure they comply with Eagle Versioner
    // standards
    if (mergeReqCommitMsgs) {
      // if merge request from forked repo branch to target development branch
      if (!gitRunner.isValidRef(env.gitlabSourceBranch.toString(), globals.get("PRODUCTION_BRANCH_REGEX"))
          && !gitRunner.isValidRef(env.gitlabSourceBranch.toString(), globals.get("DEVELOPMENT_BRANCH_REGEX"))
          && !gitRunner.isValidRef(env.gitlabSourceBranch.toString(), globals.get("TEST_BRANCH_REGEX"))
          && gitRunner.isValidRef(env.gitlabTargetBranch.toString(), globals.get("DEVELOPMENT_BRANCH_REGEX"))) {
        mergeReqCommitMsgs.each {
          if (evMandator.isVersionChangeCommit(it)) {
            error("The commit with the below message is of the version change type which is not allowed when introducing " +
              "new commits into the upstream development branch:\n\n${it}\n\nAll version change commits will be made " +
              "automatically by this Jenkins pipeline job.")
          } else if (evMandator.isWipCommit(it)) {
            error("The commit with the below message is of the WIP change type which is not allowed when introducing " +
              "new commits into the upstream development branch:\n\n${it}\n\nBe sure to use Eagle Versioner to squash " +
              "prior contiguous WIP commits.")
          } else if (evMandator.isCommitCompliant(it)) {
            echo "The commit with below message is compliant with Eagle Versioner standards:\n\n${it}"
          } else {
            error("The commit with below message isn't compliant with Eagle Versioner standards:\n\n${it}")
          }
        }
      } else {
        echo "Skipped checking the commits for this merge request since the source branch isn't from a forked repository " +
          "and the target branch isn't the upstream development branch."
      }
    } else {
      error("Merge request commit messages couldn't be retrieved from the server.")
    }
  } else {
    echo "Skipped merge request validations since the GitLab action isn't MERGE or NOTE."
  }

  jobPropsUpdater.invoke(
    globals.get("WEBHOOK_TOKEN_CRED_ID"),
    globals.get("PRODUCTION_BRANCH_REGEX"),
    globals.get("MR_NOTE_REGEX_STRING"),
    globals.get("MAX_BUILDS_TO_KEEP"),
    globals.get("SKIPPABLE_STAGES"),
    [
      "gitlabConnectionName": globals.get("GITLAB_CONNECTION_NAME"),
      "gitlabConnectionCredId": globals.get("GITLAB_API_CRED_ID")
    ],
    [
      "gitlabActionType": (env.gitlabActionType) ? env.gitlabActionType.toString() : "",
      "gitlabTargetBranch": (env.gitlabTargetBranch) ? env.gitlabTargetBranch.toString() : ""
    ],
    globals.get("VALID_BRANCHES"),
    globals.get("BUILD_TRIGGER_BRANCHES_CSV"),
    [ "exclCiSkipPushBranchesCsv": globals.get("CI_SKIP_ON_PUSH_EXCL_BRANCHES") ]
  )
}

/**
* Invokes the Secrets Scan Stage Logic.
*/
void callSecretsScanLogic() {
  // scan for secrets
  if (jobDataManager.doesFileExist(globals.get("JENKINS_CHILD_WORKSPACE_CONTAINER_PATH"),
      globals.get("GITLEAKS_CONFIG_FILE_NAME"))) {
    echo "Found the ${globals.get('GITLEAKS_CONFIG_FILE_NAME')} configuration file in the local repo " +
      "which will now be used for the GitLeaks scan."
    dockerBroker.invokeAction(
      env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
      globals.get("DOCKER_PUBLIC_REGISTRY_URL"), globals.get("DOCKER_PUBLIC_REGISTRY_CRED_ID"),
      globals.get("GITLEAKS_IMAGE_NAME"), globals.get("GITLEAKS_IMAGE_TAG"),
      globals.get("GITLEAKS_CONTAINER_OPTS"),
      "gitleaks --verbose --pretty --redact --threads=2 --repo-config --repo-path=/git-repo " +
      "--report=/git-repo/target/gitleaks-report.json"
    )
  } else {
    echo "Couldn't find the ${globals.get('GITLEAKS_CONFIG_FILE_NAME')} configuration file in the " +
      "local repo. Using default configuration for the GitLeaks scan."
    dockerBroker.invokeAction(
      env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
      globals.get("DOCKER_PUBLIC_REGISTRY_URL"), globals.get("DOCKER_PUBLIC_REGISTRY_CRED_ID"),
      globals.get("GITLEAKS_IMAGE_NAME"), globals.get("GITLEAKS_IMAGE_TAG"),
      globals.get("GITLEAKS_CONTAINER_OPTS"),
      "gitleaks --verbose --pretty --redact --threads=2 --repo-path=/git-repo " +
      "--report=/git-repo/target/gitleaks-report.json"
    )
  }
}

/**
* Invokes the Get Current Version Stage Logic.
*/
void callGetCurrentVerLogic() {
  globals.set("CURRENT_VERSION",
    dockerBroker.invokeAction(
      env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
      globals.get("DOCKER_PRIVATE_REGISTRY_URL"), globals.get("DOCKER_PRIVATE_REGISTRY_CRED_ID"),
      globals.get("MAVEN_IMAGE_NAME"), globals.get("MAVEN_IMAGE_TAG"),
      globals.get("MAVEN_CONTAINER_OPTS"),
      "/usr/local/bin/mvn-entrypoint.sh mvn org.apache.maven.plugins:maven-help-plugin:3.2.0:evaluate " +
      "-Dexpression=project.version -q -DforceStdout")
    )

  echo "The current version is ${globals.get('CURRENT_VERSION')}."
}

/**
* Invokes the Calculate Version Stage Logic.
*/
void callCalcVerLogic() {
  // must switch to the name of the branch instead of the commit SHA (commit SHA used for checkout by
  // the Git SCM plugin even though the branch triggered the push event) or HEAD reference in order
  // for Eagle Versioner to work
  if (env.gitlabActionType.toString() == "PUSH" && env.gitlabBranch) {
    sh "git checkout ${gitlabBranch}"
  } else if ((env.gitlabActionType.toString() == "MERGE" || env.gitlabActionType.toString() == "NOTE")
      && env.gitlabTargetBranch) {
    sh "git checkout -b merge-req-${gitlabMergeRequestId}"
  } else {
    sh "git checkout ${globals.get('MANUAL_JOB_INVOCATION_BRANCH')}"
  }

  globals.set("NEW_VERSION",
    dockerBroker.invokeAction(
      env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
      globals.get("DOCKER_PRIVATE_REGISTRY_URL"), globals.get("DOCKER_PRIVATE_REGISTRY_CRED_ID"),
      globals.get("EAGLE_VERSIONER_IMAGE_NAME"), globals.get("EAGLE_VERSIONER_IMAGE_TAG"),
      globals.get("EAGLE_VERSIONER_CONTAINER_OPTS"), "ev calculate --dev-appendage snapshot"
    )
  )

  echo "The calculated version is ${globals.get('NEW_VERSION')}."
}

/**
* Invokes the Update Version Stage Logic.
*/
void callUpdateVerLogic() {
  dockerBroker.invokeAction(
    env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
    globals.get("DOCKER_PRIVATE_REGISTRY_URL"), globals.get("DOCKER_PRIVATE_REGISTRY_CRED_ID"),
    globals.get("MAVEN_IMAGE_NAME"), globals.get("MAVEN_IMAGE_TAG"),
    globals.get("MAVEN_CONTAINER_OPTS"),
    "/usr/local/bin/mvn-entrypoint.sh mvn versions:set versions:commit -DnewVersion=${globals.get('NEW_VERSION')}"
  )

  dockerBroker.invokeAction(
    env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
    globals.get("DOCKER_PRIVATE_REGISTRY_URL"), globals.get("DOCKER_PRIVATE_REGISTRY_CRED_ID"),
    globals.get("EAGLE_VERSIONER_IMAGE_NAME"), globals.get("EAGLE_VERSIONER_IMAGE_TAG"),
    globals.get("EAGLE_VERSIONER_CONTAINER_OPTS"),
    "git add pom.xml && ev commit --manual --dev-appendage snapshot --change-type version_change " +
    "--new-version ${globals.get('NEW_VERSION')} --insert-skip-ci-tag"
  )
}

/**
* Invokes the Generate Changelog Stage Logic.
*/
void callGenChangelogLogic() {
  dockerBroker.invokeAction(
    env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
    globals.get("DOCKER_PRIVATE_REGISTRY_URL"), globals.get("DOCKER_PRIVATE_REGISTRY_CRED_ID"),
    globals.get("EAGLE_VERSIONER_IMAGE_NAME"), globals.get("EAGLE_VERSIONER_IMAGE_TAG"),
    globals.get("EAGLE_VERSIONER_CONTAINER_OPTS"),
    "ev changelog && git add CHANGELOG.md && ev commit --manual --change-type changelog --insert-skip-ci-tag " +
    "--short-msg \"Updated the Changelog\""
  )
}

/**
* Invokes the Build Stage Logic.
*/
void callBuildLogic() {
  echo "Building the application artifacts using Maven..."

  dockerBroker.invokeAction(
    env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
    globals.get("DOCKER_PRIVATE_REGISTRY_URL"), globals.get("DOCKER_PRIVATE_REGISTRY_CRED_ID"),
    globals.get("MAVEN_IMAGE_NAME"), globals.get("MAVEN_IMAGE_TAG"),
    globals.get("MAVEN_CONTAINER_OPTS"), "/usr/local/bin/mvn-entrypoint.sh mvn install -DskipTests"
  )

  if (jobDataManager.doesFileExist(globals.get("JENKINS_CHILD_WORKSPACE_CONTAINER_PATH"), ".dockerignore")
      && jobDataManager.doesFileExist(globals.get("JENKINS_CHILD_WORKSPACE_CONTAINER_PATH"), "Dockerfile")) {
    if ((env.gitlabActionType && env.gitlabActionType.toString() == "PUSH") || env.gitlabActionType == null
        && env.chosenBranch) {
      dockerBroker.buildImage(
        env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
        globals.get("DOCKER_PUBLIC_REGISTRY_URL"), globals.get("DOCKER_PUBLIC_REGISTRY_CRED_ID"),
        globals.get("DOCKER_IMAGE_NAME"), globals.get("NEW_VERSION")
      )
    } else if (env.gitlabActionType && (env.gitlabActionType.toString() == "MERGE" || env.gitlabActionType.toString() == "NOTE")) {
      dockerBroker.buildImage(
        env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
        globals.get("DOCKER_PUBLIC_REGISTRY_URL"), globals.get("DOCKER_PUBLIC_REGISTRY_CRED_ID"),
        globals.get("DOCKER_IMAGE_NAME"), "latest"
      )
    }
  } else {
    error("The following files are required to build a Docker image: .dockerignore and Dockerfile.")
  }
}

/**
* Invokes the Test Stage Logic.
*/
void callTestLogic() {
  echo "Running tests using Maven..."

  // requires the JaCoCo Maven Plugin configured in pom.xml
  // - see https://natritmeyer.com/howto/reporting-aggregated-unit-and-integration-test-coverage-with-jacoco/
  dockerBroker.invokeAction(
    env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
    globals.get("DOCKER_PRIVATE_REGISTRY_URL"), globals.get("DOCKER_PRIVATE_REGISTRY_CRED_ID"),
    globals.get("MAVEN_IMAGE_NAME"), globals.get("MAVEN_IMAGE_TAG"),
    globals.get("MAVEN_CONTAINER_OPTS"), "/usr/local/bin/mvn-entrypoint.sh mvn verify"
  )

  // TODO: insert system tests here such as Selenium, etc.
  /*try {

  } catch (Exception exception) {
    // send email stating the system tests failed but don't fail the current job instance
    // so developers aren't held up
  }*/
}

/**
* Invokes the Lint Stage Logic.
*/
void callLintLogic() {
  echo "Running linting actions..."

  if (jobDataManager.doesFileExist(globals.get("JENKINS_CHILD_WORKSPACE_CONTAINER_PATH"),
      globals.get("HADOLINT_CONFIG_FILE_NAME"))) {
    echo "Found the ${globals.get('HADOLINT_CONFIG_FILE_NAME')} configuration file in the local repo " +
      "which will now be used for the Hadolint scan."

    // validate Hadolint config file
    jobDataManager.validateHadolintConfigFile(globals.get("JENKINS_CHILD_WORKSPACE_CONTAINER_PATH"),
      globals.get("HADOLINT_CONFIG_FILE_NAME"), globals.get("DOCKER_TRUSTED_REGISTRIES"),
      globals.get("DOCKERFILE_LINTER_IGNORED_RULES"))
  } else {
    echo "Did not find the ${globals.get('HADOLINT_CONFIG_FILE_NAME')} configuration file in the " +
      "local repo. The default configuration will be used."
  }

  dockerBroker.invokeAction(
    env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
    globals.get("DOCKER_PUBLIC_REGISTRY_URL"), globals.get("DOCKER_PUBLIC_REGISTRY_CRED_ID"),
    globals.get("HADOLINT_IMAGE_NAME"), globals.get("HADOLINT_IMAGE_TAG"),
    globals.get("HADOLINT_CONTAINER_OPTS"), "hadolint Dockerfile"
  )
}

/**
* Invokes the OWASP Dependency Check Stage Logic.
*/
void callOwaspDepCheckLogic() {
  echo "Running OWASP Dependency Check scan..."

  if (jobDataManager.doesFileExist(globals.get("JENKINS_CHILD_WORKSPACE_CONTAINER_PATH"),
      globals.get("OWASP_DEP_CHECK_SUPPRESSION_FILE_NAME"))) {
    dockerBroker.invokeAction(
      env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
      globals.get("DOCKER_PUBLIC_REGISTRY_URL"), globals.get("DOCKER_PUBLIC_REGISTRY_CRED_ID"),
      globals.get("OWASP_DEP_CHECK_IMAGE_NAME"), globals.get("OWASP_DEP_CHECK_IMAGE_TAG"),
      globals.get("OWASP_DEP_CHECK_CONTAINER_OPTS"),
      "/usr/share/dependency-check/bin/dependency-check.sh --scan " +
      "${globals.get('OWASP_DEP_CHECK_WORKSPACE_CONTAINER_PATH')} --format \"ALL\" " +
      "--project \"${globals.get('APPLICATION_NAME')}\" --out /src/target/owasp-dep-check-report " +
      "--failOnCVSS ${globals.get('OWASP_DEP_CHECK_FAIL_SCORE')} " +
      "--suppression /src/${globals.get('OWASP_DEP_CHECK_SUPPRESSION_FILE_NAME')}"
    )
  } else {
    dockerBroker.invokeAction(
      env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
      globals.get("DOCKER_PUBLIC_REGISTRY_URL"), globals.get("DOCKER_PUBLIC_REGISTRY_CRED_ID"),
      globals.get("OWASP_DEP_CHECK_IMAGE_NAME"), globals.get("OWASP_DEP_CHECK_IMAGE_TAG"),
      globals.get("OWASP_DEP_CHECK_CONTAINER_OPTS"),
      "/usr/share/dependency-check/bin/dependency-check.sh --scan " +
      "${globals.get('OWASP_DEP_CHECK_WORKSPACE_CONTAINER_PATH')} --format \"ALL\" " +
      "--project \"${globals.get('APPLICATION_NAME')}\" --out /src/target/owasp-dep-check-report " +
      "--failOnCVSS ${globals.get('OWASP_DEP_CHECK_FAIL_SCORE')}"
    )
  }
}

/**
* Invokes the SonarQube Scan Stage Logic.
*/
void callSonarQubeScanLogic() {
  echo "Running SonarQube scan using Maven..."

  withSonarQubeEnv("SonarQube") {
    withCredentials([string(credentialsId: globals.get("SONARQUBE_CRED_ID"), variable: "accessToken")]) {
      // requires the SonarQube Maven Plugin configured in pom.xml
      // - see https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-maven/
      dockerBroker.invokeAction(
        env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
        globals.get("DOCKER_PRIVATE_REGISTRY_URL"), globals.get("DOCKER_PRIVATE_REGISTRY_CRED_ID"),
        globals.get("MAVEN_IMAGE_NAME"), globals.get("MAVEN_IMAGE_TAG"),
        globals.get("MAVEN_CONTAINER_OPTS"),
        "/usr/local/bin/mvn-entrypoint.sh mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar " +
        "-Dsonar.host.url=${globals.get('SONARQUBE_INTERNAL_ROOT_URL')} -Dsonar.scm.disabled=true " +
        "-Dsonar.projectKey=${globals.get('SONARQUBE_PROJECT_ID')} -Dsonar.login=${accessToken} " +
        "-Dsonar.dependencyCheck.xmlReportPath=${globals.get('MAVEN_WORKSPACE_CONTAINER_PATH')}" +
        "/target/owasp-dep-check-report/dependency-check-report.xml " +
        "-Dsonar.dependencyCheck.jsonReportPath=${globals.get('MAVEN_WORKSPACE_CONTAINER_PATH')}" +
        "/target/owasp-dep-check-report/dependency-check-report.json " +
        "-Dsonar.dependencyCheck.htmlReportPath=${globals.get('MAVEN_WORKSPACE_CONTAINER_PATH')}" +
        "/target/owasp-dep-check-report/dependency-check-report.html"
      )
    }

    // don't continue unless the quality gate has passed
    sonarQubeScanner.waitForResults(
      globals.get("JENKINS_CHILD_WORKSPACE_CONTAINER_PATH"),
      globals.get("SONARQUBE_INTERNAL_ROOT_URL"),
      globals.get("SONARQUBE_CRED_ID"),
      globals.get("SONARQUBE_QUERY_INTERVAL")
    )
  }
}

/**
* Invokes the Version/Changelog Push Stage Logic.
*/
void callVerAndClPushLogic() {
  echo "Pushing version and changelog commits to upstream repository..."

  gitRunner.pushCommits(
    globals.get("GITLAB_INTERNAL_ROOT_URL"),
    [
      "gitServerCredentialsId" : globals.get("GITLAB_USER_CRED_ID"),
      "gitServerApiTokenCredId": globals.get("GITLAB_API_CURL_CRED_ID")
    ],
    [
      "userFullName"    : globals.get("CICD_ADMIN_NAME"),
      "userEmailAddress": globals.get("CICD_ADMIN_EMAIL")
    ],
    globals.get("GIT_UPSTREAM_INTERNAL_REPO_URL")
  )

  if ((env.gitlabBranch && gitRunner.isValidRef(env.gitlabBranch.toString(), globals.get("PRODUCTION_BRANCH_REGEX")))
      || (env.gitlabActionType == null && env.chosenBranch
      && gitRunner.isValidRef(env.chosenBranch.toString(), globals.get("PRODUCTION_BRANCH_REGEX"))
      && env.deployOnly.toBoolean() == false)) {
    echo "Creating and pushing the production tag v${globals.get('NEW_VERSION')}..."

    gitRunner.createTag(
      globals.get("GITLAB_INTERNAL_ROOT_URL"),
      [
        "gitServerCredentialsId" : globals.get("GITLAB_USER_CRED_ID"),
        "gitServerApiTokenCredId": globals.get("GITLAB_API_CURL_CRED_ID")
      ],
      [
        "userFullName"    : globals.get("CICD_ADMIN_NAME"),
        "userEmailAddress": globals.get("CICD_ADMIN_EMAIL")
      ],
      "v" + globals.get("NEW_VERSION")
    )

    gitRunner.pushTag(
      globals.get("GITLAB_INTERNAL_ROOT_URL"),
      [
        "gitServerCredentialsId" : globals.get("GITLAB_USER_CRED_ID"),
        "gitServerApiTokenCredId": globals.get("GITLAB_API_CURL_CRED_ID")
      ],
      [
        "userFullName"    : globals.get("CICD_ADMIN_NAME"),
        "userEmailAddress": globals.get("CICD_ADMIN_EMAIL")
      ],
      globals.get("GIT_UPSTREAM_INTERNAL_REPO_URL"),
      "v" + globals.get("NEW_VERSION")
    )
  } else {
    echo "Skipped creating and pushing the tag since that doesn't apply to non-production branches."
  }
}

/**
* Invokes the Build Artifacts Push Stage Logic.
*/
void callBuildArtifPushLogic() {
  echo "Pushing build artifacts..."

  dockerBroker.invokeAction(
    env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
    globals.get("DOCKER_PRIVATE_REGISTRY_URL"), globals.get("DOCKER_PRIVATE_REGISTRY_CRED_ID"),
    globals.get("MAVEN_IMAGE_NAME"), globals.get("MAVEN_IMAGE_TAG"),
    globals.get("MAVEN_CONTAINER_OPTS"), "/usr/local/bin/mvn-entrypoint.sh mvn deploy -DskipTests " +
    "-DaltSnapshotDeploymentRepository=nexus::default::${globals.get('MAVEN_INTERNAL_SNAPSHOT_REPO_URL')} " +
    "-DaltReleaseDeploymentRepository=nexus::default::${globals.get('MAVEN_INTERNAL_RELEASE_REPO_URL')}"
  )

  dockerBroker.pushImage(
    env.CLOUD_HOST_IP.toString(), env.CLOUD_HOST_CREDS_ID.toString(),
    globals.get("DOCKER_PRIVATE_REGISTRY_URL"), globals.get("DOCKER_PRIVATE_REGISTRY_CRED_ID"),
    globals.get("DOCKER_IMAGE_NAME"), globals.get("NEW_VERSION")
  )
}

/**
* Invokes the Application Deployment Stage Logic.
*/
void callAppDeploymentLogic() {
  if ((env.gitlabBranch && gitRunner.isValidRef(env.gitlabBranch.toString(), globals.get("DEVELOPMENT_BRANCH_REGEX")))
      || (env.chosenBranch && gitRunner.isValidRef(env.chosenBranch.toString(), globals.get("DEVELOPMENT_BRANCH_REGEX")))) {
    echo "Deploying application to development environment..."
    // TODO: implement this
  } else if ((env.gitlabBranch && gitRunner.isValidRef(env.gitlabBranch.toString(), globals.get("TEST_BRANCH_REGEX")))
      || (env.chosenBranch && gitRunner.isValidRef(env.chosenBranch.toString(), globals.get("TEST_BRANCH_REGEX")))) {
    echo "Deploying application to test environment..."
    // TODO: implement this
  } else if ((env.gitlabBranch && gitRunner.isValidRef(env.gitlabBranch.toString(), globals.get("PRODUCTION_BRANCH_REGEX")))
      || (env.chosenBranch && gitRunner.isValidRef(env.chosenBranch.toString(), globals.get("PRODUCTION_BRANCH_REGEX")))) {
    echo "Deploying application to production environment..."
    // TODO: implement this
  }
}
// ---------------------------------------------------------------------------------------------------------------------
// </Methods>

// <Stages>
// ---------------------------------------------------------------------------------------------------------------------
// lock so no other job instances can run until the current one completes - this prevents possible concurrency errors
lock(env.JOB_NAME.toString()) {
  // This will automatically choose a node based on the YADP settings configured in Jenkins.
  // See https://github.com/KostyaSha/yet-another-docker-plugin for more info on how this works.
  node {
    updateGitlabCommitStatus(name: "build", state: "running")

    timeout(time: globals.get("JOB_INSTANCE_TIMEOUT_MINS"), unit: "MINUTES") {
      try {
        stage("Prepare and Enforce") {
          gitlabCommitStatus(env.STAGE_NAME.toString()) {
            callPrepareAndEnforceLogic()
          }
        }

        // change to created job instance directory
        dir(globals.get("JENKINS_CHILD_WORKSPACE_CONTAINER_PATH")) {
          stage("Checkout Code") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "PUSH"
                  || env.gitlabActionType.toString() == "MERGE" || env.gitlabActionType.toString() == "NOTE"))
                  || (env.gitlabActionType == null && env.chosenBranch && env.deployOnly.toBoolean() == false))) {
                // wrap stage logic in retry block to enable retries for failed stage
                retry(globals.get("CHECKOUT_CODE_STAGE_RETRY_COUNT")) {
                  gitRunner.autoCheckout()

                  // set local Git user info so future Git commands will work correctly
                  sh "git config --local user.name \"${globals.get('CICD_ADMIN_NAME')}\" " +
                    "&& git config --local user.email ${globals.get('CICD_ADMIN_EMAIL')}"
                }
              }
            }
          }

          stage("Validate Artifacts") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "PUSH"
                  || env.gitlabActionType.toString() == "MERGE" || env.gitlabActionType.toString() == "NOTE"))
                  || (env.gitlabActionType == null && env.chosenBranch && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("VALIDATE_ARTIFACTS_STAGE_RETRY_COUNT")) {
                  validator.checkArtifacts()
                }
              }
            }
          }

          stage("Secrets Scan") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "MERGE"
                  || env.gitlabActionType.toString() == "NOTE")) || (env.gitlabActionType == null && env.chosenBranch
                  && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("SECRETS_SCAN_STAGE_RETRY_COUNT")) {
                  callSecretsScanLogic()
                }
              }
            }
          }

          stage("Get Current Version") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "PUSH"
                  || env.gitlabActionType.toString() == "MERGE" || env.gitlabActionType.toString() == "NOTE"))
                  || (env.gitlabActionType == null && env.chosenBranch && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("GET_CURRENT_VERSION_STAGE_RETRY_COUNT")) {
                  callGetCurrentVerLogic()
                }
              }
            }
          }

          stage("Calculate Version") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "PUSH"
                  || env.gitlabActionType.toString() == "MERGE" || env.gitlabActionType.toString() == "NOTE"))
                  || (env.gitlabActionType == null && env.chosenBranch && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("CALC_VERSION_STAGE_RETRY_COUNT")) {
                  callCalcVerLogic()
                }
              }
            }
          }

          stage("Update Version") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && globals.get("NEW_VERSION") && globals.get("CURRENT_VERSION")
                  && globals.get("CURRENT_VERSION") != globals.get("NEW_VERSION")
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "PUSH"
                  || env.gitlabActionType.toString() == "MERGE" || env.gitlabActionType.toString() == "NOTE"))
                  || (env.gitlabActionType == null && env.chosenBranch && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("UPDATE_VERSION_STAGE_RETRY_COUNT")) {
                  callUpdateVerLogic()
                }
              }
            }
          }

          stage("Generate Changelog") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && globals.get("NEW_VERSION") && globals.get("CURRENT_VERSION")
                  && globals.get("CURRENT_VERSION") != globals.get("NEW_VERSION")
                  && ((env.gitlabActionType && env.gitlabActionType.toString() == "PUSH")
                  || (env.gitlabActionType == null && env.chosenBranch && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("CHANGELOG_GEN_STAGE_RETRY_COUNT")) {
                  callGenChangelogLogic()
                }
              }
            }
          }

          stage("Build") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "PUSH"
                  || env.gitlabActionType.toString() == "MERGE" || env.gitlabActionType.toString() == "NOTE"))
                  || (env.gitlabActionType == null && env.chosenBranch && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("BUILD_STAGE_RETRY_COUNT")) {
                  callBuildLogic()
                }
              }
            }
          }

          stage("Test") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "MERGE"
                  || env.gitlabActionType.toString() == "NOTE")) || (env.gitlabActionType == null && env.chosenBranch
                  && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("TEST_STAGE_RETRY_COUNT")) {
                  callTestLogic()
                }
              }
            }
          }

          stage("Lint") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "MERGE"
                  || env.gitlabActionType.toString() == "NOTE")) || (env.gitlabActionType == null && env.chosenBranch
                  && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("LINT_STAGE_RETRY_COUNT")) {
                  callLintLogic()
                }
              }
            }
          }

          stage("OWASP Dependency Check") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "MERGE"
                  || env.gitlabActionType.toString() == "NOTE")) || (env.gitlabActionType == null && env.chosenBranch
                  && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("OWASP_DEP_CHECK_STAGE_RETRY_COUNT")) {
                  callOwaspDepCheckLogic()
                }
              }
            }
          }

          stage("SonarQube Scan") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "MERGE"
                  || env.gitlabActionType.toString() == "NOTE")) || (env.gitlabActionType == null && env.chosenBranch
                  && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("SONARQUBE_SCANNING_STAGE_RETRY_COUNT")) {
                  callSonarQubeScanLogic()
                }
              }
            }
          }

          stage("Veracode Scan") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && (env.gitlabActionType.toString() == "MERGE"
                  || env.gitlabActionType.toString() == "NOTE")) || (env.gitlabActionType == null && env.chosenBranch
                  && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("VERACODE_SCANNING_STAGE_RETRY_COUNT")) {
                  echo "Running Veracode scan..."
                  veracodeScanner.invoke(globals.get("VERACODE_CRED_ID"), globals.get("VERACODE_SCAN_OPTIONS"))
                }
              }
            }
          }

          stage("Version/Changelog Push") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && globals.get("NEW_VERSION") && globals.get("CURRENT_VERSION")
                  && globals.get("CURRENT_VERSION") != globals.get("NEW_VERSION")
                  && ((env.gitlabActionType && env.gitlabActionType.toString() == "PUSH")
                  || (env.gitlabActionType == null && env.chosenBranch && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("VERSION_CHANGELOG_PUSH_STAGE_RETRY_COUNT")) {
                  callVerAndClPushLogic()
                }
              }
            }
          }

          stage("Build Artifacts Push") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && env.gitlabActionType.toString() == "PUSH")
                  || (env.gitlabActionType == null && env.chosenBranch && env.deployOnly.toBoolean() == false))) {
                retry(globals.get("PUSH_BUILD_ARTIFACTS_STAGE_RETRY_COUNT")) {
                  callBuildArtifPushLogic()
                }
              }
            }
          }

          stage("App Deployment") {
            gitlabCommitStatus(env.STAGE_NAME.toString()) {
              when(globals.get("STAGES_TO_SKIP").contains(env.STAGE_NAME.toString()) == false
                  && ((env.gitlabActionType && env.gitlabActionType.toString() == "PUSH")
                  || (env.gitlabActionType == null && env.chosenBranch && env.deployOnly.toBoolean()))) {
                retry(globals.get("APP_DEPLOYMENT_STAGE_RETRY_COUNT")) {
                  callAppDeploymentLogic()
                }
              }
            }
          }
        }
      } catch (Exception exception) {
        // print exception details
        println(exception.toString())
        println(exception.getMessage())

        // if this variable is set, the job instance was successful - not using currentBuild.result or
        // currentBuild.currentResult as it isn't accurate at the time of checking it
        globals.set("BUILD_RESULT", "FAILURE")
      } finally {
        finalizer.invoke()
      }
    }
  }
}
// ---------------------------------------------------------------------------------------------------------------------
// </Stages>