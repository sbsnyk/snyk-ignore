# Snyk Ignore

## What is this?
Currently, customers can [ignore](https://docs.snyk.io/scan-using-snyk/find-and-manage-priority-issues/ignore-issues) a Snyk issue via the Web UI. However, security teams at larger enterprises might not always want their developers to do this without first providing a valid reason. Ideally, Snyk would allow developers to first "request an ignore" and enable the security team to review it before accepting or rejecting it. This CLI tool facilitates such a process with the help of JIRA:
1. Developers may ask the security team to ignore an issue by creating a JIRA ticket for each unwanted issue via Snyk's [JIRA integration](https://docs.snyk.io/integrate-with-snyk/notification-and-ticketing-systems-integrations/jira-integration). (Note that in this scenario only the security team should have the permission to ignore issues, which can be enforced via [Member Roles](https://docs.snyk.io/snyk-admin/manage-permissions-and-roles/manage-member-roles)).
2. A discussion can then take place within the JIRA ticket during which developers may present their arguments for why they believe the issue needs to be ignored.
3. If the security team agrees or disagrees, they will transition the ticket's state to `Accepted` or `Rejected`, respectively.
4. The team will then run the `sync` subcommand, which will automatically apply those ignores that have been approved (inserting the reasons stated in the ticket as ignore comments).
5. Optionally, the team can generate an Html report via the `--report` flag, which will show the outstanding tickets to be reviewed, as well as the already approved ones. Apart from being a worklist, the report may also satisfy some of the customer's compliance obligations.
