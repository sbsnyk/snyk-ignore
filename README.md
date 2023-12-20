# Snyk Ignore

## What is this?
Currently, customers can [ignore](https://docs.snyk.io/scan-using-snyk/find-and-manage-priority-issues/ignore-issues) a Snyk issue via the Web UI. However, security teams at larger enterprises might not always want their developers to do this without first stating a valid reason. Ideally, Snyk would allow developers to first "request an ignore" and enable the security team to review it before accepting or rejecting it. This tool facilitates such a process with the help of JIRA:
1. Developers may prompt the security team to ignore an issue by creating a JIRA ticket via Snyk's [JIRA integration](https://docs.snyk.io/integrate-with-snyk/notification-and-ticketing-systems-integrations/jira-integration). (Note that in this scenario only the security team should have the permission to ignore issues, which can be enforced via [Member Roles](https://docs.snyk.io/snyk-admin/manage-permissions-and-roles/manage-member-roles)).
2. A discussion can then take place within the JIRA ticket during which developers may present their arguments in support of the requested ignore.
3. If the security team agrees or disagrees, they will transition the ticket's state to `Accepted` or `Rejected`, respectively.
4. The team will then run the `sync` subcommand, which will automatically apply those ignores that have been approved (inserting the reasons stated in the ticket as ignore comments).
5. Optionally, the team can generate an Html report via the `--report` flag, which will list the outstanding tickets to be reviewed, as well as the approved ones. Apart from being a worklist, the report may also satisfy some of the customer's compliance obligations.


## Requirements

* Python 3.11 (An older version will likely work as well, but the script was tested with this version)
* A [Snyk API token](https://docs.snyk.io/getting-started/how-to-obtain-and-authenticate-with-your-snyk-api-token) with the necessary permissions to list issues and apply ignores. It should also be a member of all the Snyk organizations for which issues will be ignored.
* A JIRA user name (usually an email!) and API token
* A JIRA project set up to work with [Snyk's JIRA integration](https://docs.snyk.io/integrate-with-snyk/notification-and-ticketing-systems-integrations/jira-integration).
* The project must have a [JIRA workflow](https://www.atlassian.com/software/jira/guides/workflows/overview#what-is-a-jira-workflow) with the following states (Other workflows are possible, but they will require minor changes in the tool):
<img src="./workflow.png" width="300">

## Environment Variables

The tool expects the following environment variables:
* `SNYK_TOKEN` (the Snyk API token)
* `JIRA_USER` (the JIRA user account name, usually an email)
* `JIRA_TOKEN` (the JIRA API token)

## Usage

```shell
$ ./snyk-ignore sync -h
usage: snyk-ignore sync [-h] -p JIRA_PROJECT [-o REPORT] -j JIRA_HOST
                        [-s {snyk.io,eu.snyk.io,au.snyk.io}]

Synchronize JIRA tickets and their linked Snyk issues

options:
  -h, --help            show this help message and exit
  -p JIRA_PROJECT, --jira-project JIRA_PROJECT
                        The project key of the JIRA project to synchronize,
                        e.g. "ISSUES"
  -o REPORT, --report REPORT
                        The path to were the HTML report ought to be saved
  -j JIRA_HOST, --jira-host JIRA_HOST
                        The host of the JIRA instance, e.g.:
                        "mycompany.atlassian.net"
  -s {snyk.io,eu.snyk.io,au.snyk.io}, --snyk-host {snyk.io,eu.snyk.io,au.snyk.io}
                        The host of the Snyk tenant, e.g.: "snyk.io" or
                        "eu.snyk.io" or "au.snyk.io".
```

## Examples

* Synchronize the tickets in the JIRA project with project key `IGNORE` located in the JIRA instance at `initech.atlassian.net` with the Snyk EU tenant. Also generate a report at `report.html`:
```shell
./snyk-ignore sync --jira-project IGNORE --report report.html --jira-host 'initech.atlassian.net' --snyk-host 'eu.snyk.io' 
```

## Screenshots
### Report
![example report](./example-report.png)

### Output
```INFO: Processing ticket IGNORE-18...
$ ./snyk-ignore sync --jira-project IGNORE --report report.html --jira-host 'initech.atlassian.net' --snyk-host 'eu.snyk.io
INFO: Processing ticket IGNORE-17...
INFO: Processing ticket IGNORE-16...
INFO: Processing ticket IGNORE-15...
INFO: Processing ticket IGNORE-14...
INFO: Processing ticket IGNORE-12...
INFO: Processing ticket IGNORE-11...
INFO: Processing ticket IGNORE-10...
INFO: Processing ticket IGNORE-9...
INFO: Processing ticket IGNORE-8...
INFO: Processing ticket IGNORE-7...
INFO: Processing ticket IGNORE-6...
INFO: Processing ticket IGNORE-5...
WARN: Ticket "IGNORE-5" contains no link to a Snyk issue and will hence be ignored!
INFO: Processing ticket IGNORE-3...shell
```
