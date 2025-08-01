@Library('cicid-shared-lib@checkov-priyanshu') _
def cipipeline = new opstree.ci.templates.java_ci.java_ci()

node {
  cipipeline.call([

    // WORKSPACE MANAGEMENT
    clean_workspace: true,
    ignore_clean_workspace_failure: false,
    delete_dirs: false,
    clean_when_build_aborted: true,
    clean_when_build_failed: true,
    clean_when_not_built: true,
    clean_when_build_succeed: true,
    clean_when_build_unstable: true,

    // VCS MANAGEMENT 
    repo_https_url: "https://github.com/Priyanshu498/Tomcat-001.git",
    repo_ssh_url: "https://github.com/Priyanshu498/Tomcat-001.git",
    repo_branch: "main",
    repo_url_type: "http",
    jenkins_git_creds_id: "Priyanshu498",
    source_code_path: "",

    // Dependency Scanning
    dependency_check: false,
    dependency_scan_tool: "owasp",
    owasp_project_name: "owasp",
    owasp_report_publish: true,
    owasp_report_format: "html",
    fail_job_if_dependency_returned_exception: true,

    // Creds Scanning
    gitleaks_check: false,
    fail_job_if_leak_detected: false,
    gitleaks_report_format: "json",
    gitleaks_report_jenkins_publish: true,

    // ✅ IaC Security Scanning with Checkov
    checkov_check: true,
    checkov_report_format: "json",                // can be json, sarif, cli, etc.
    checkov_report_jenkins_publish: true,
    fail_job_if_checkov_failed: false,            // set to true if you want build to fail on Checkov findings

    // Code Build
    perform_code_build: false,
    build_tool: "maven",
    pom_location: " ",

    // Unit Testing
    unit_testing_check: false,
    fail_job_if_unit_issue_detected: false,
    build_tool: "maven",
    unit_test_reports_path: "*/target/surefire-reports/*.xml",
    findbugs_test_report_path: "*/target/findbugs/*.xml",
    withmaven_globaltool_jdk: "",
    withmaven_globaltool_maven: "",
    mvn_settings_path: "settings.xml",

    // Static Code Analysis
    codebase_to_scan_directory: "**",
    static_code_analysis_check: false,
    path_to_sonar_properties: "sonar.properties",
    fail_job_if_analysis_returned_exception: false,
    jenkins_sonarqube_token_creds_id: "sonar-token",

    // Build Dockerfile
    perform_build_dockerfile: false,
    image_name: "lastmile",
    dockerfile_location: "",
    dockerfile_context: "",
    codeartifact_dependency: true,
    

    // Image scanning
    image_scanning_check: false,
    image_tag: "latest",
    scan_severity: "CRITICAL",
    image_scanning_report_publish: true,

    // Image size validator
    image_size_validator_check: false,
    max_allowed_image_size: 100,
    fail_job_if_validation_fail: false,

    // Artifact Publish
    artifact_publish_check: false,
    artifact_destination_type: "ecr",
    jenkins_aws_credentials_id: "aws-rajat",
    docker_image_name: "lastmile",
    ecr_repo_name: "lastmile",
    ecr_region: "ap-south-1",
    account_id: "543339517346",

    // Notification
    notification_enabled: false,
    notification_channel: "teams",
    webhook_url_creds_id: "teams_webhook"
  ])
}
