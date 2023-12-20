# Snyk Ignore

## What is this?
Currently, customers can ignore Snyk issue via the UI. However, security teams at larger enterprises might not always want their developers to be able to ignore issues without providing valid reasons. Ideally, Snyk would allow developers to "request an ignore" for a particular issue. This CLI tool facilitates such a process with the help of JIRA:
1. Developers may ask the security team to ignore an issue by creating a JIRA ticket for each unwanted issue via Snyk's [JIRA integration](https://docs.snyk.io/integrate-with-snyk/notification-and-ticketing-systems-integrations/jira-integration). (Note that only the security team should have the permission to ignore issues. This can be enforced via [Member Roles](https://docs.snyk.io/snyk-admin/manage-permissions-and-roles/manage-member-roles).
2. A discussion can take place within the JIRA ticket to present the reasons for why the developer thinks the issue is worthy to be ignored.
3. If the security team agrees or disagrees, they will transition the ticket's state to `Accepted` or `Rejected`, respectively.
4. The team will then run the `sync` subcommand of this tool, which will automatically apply those ignores which have been approved (including the reason stated in the ticket).
5. Optionally, the team can generate an Html report via the `--report` flag, which will show the outstanding tickets to be reviewed, as well as the already approved ones.
