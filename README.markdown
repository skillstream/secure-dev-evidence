Secure Development Evidence Pack
--------------------------------

This resource shares some general information about Skillstream's software development lifecycle, with particular reference to our security practices, and links to evidence for anyone undertaking due diligence.

The lifecycle is backed by a variant of the ['git-flow'](https://nvie.com/posts/a-successful-git-branching-model/) branching strategy, with [Atlassian Jira](https://www.atlassian.com/software/jira) tracking features and issues, and [GitHub](https://github.com/) supporting peer review, test automation and release tracking.


### List of Tools

#### Day-to-day
Below is a list of tools we use during development to assist us, backed by [Github Actions](https://github.com/features/actions) as laid-out in this [sample workflow](samples/github-workflow-ruby.yml).

* [bundler-audit](https://github.com/rubysec/bundler-audit#readme) checks 3rd-party libraries against the CVE database for known vulnerabilities in Ruby gems.
  - Evidence: [sample output from bundle audit](samples/bundle-audit.out)
* [yarn-audit](https://classic.yarnpkg.com/en/docs/cli/audit/) checks 3rd-party libraries against the CVE database for known vulnerabilities in nvm packages.
  - Evidence: [sample output from yarn audit](samples/yarn-audit.out)
* [Brakeman](https://brakemanscanner.org/) analyses projects which use the Ruby on Rails framework for common vulnerabilities.
  - Brief outline of how Brakeman helps test against some of the OWASP Top 10, from [Stack Overflow](https://stackoverflow.com/a/35229152/418602)
  - Evidence: [sample output from Brakeman](samples/brakeman.out)
* [Rubocop](https://rubocop.org/) enforces in-house coding style and is also good at highlighting the subset of bugs which can arise from violating conventions, e.g. variable shadowing, method duplication after a merge, etc.
  - Evidence: [sample output from Rubocop](samples/rubocop.out)
* [RSpec](https://rspec.info/) is used to define expected behaviour and test those expectations. Some of those expectations explicitly cover access control, but other tests will prevent the many bugs that could also have had security implications.
  - Evidence: [sample output from RSpec](samples/rspec.out)
  - Supporting tools:
    * [FactoryBot](https://github.com/thoughtbot/factory_bot) for churning-out randomised test objects
    * [Faker](https://github.com/faker-ruby/faker) for generating realistic test data
    * [Capybara](http://teamcapybara.github.io/capybara/) for front-end integration testing



#### Penetration testing
These are the main tools we use for periodic review of application security:

* [OWASP ZAP](https://www.zaproxy.org/) to scan our web apps for vulnerabilities.
* [Kali Linux VM](https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/) because it comes pre-installed with ZAP and a lot of other useful tools, and is a safe environment for handling malware if required.

##### Evidence
* Here are [some screenshots of ZAP in use](screenshots/zap.markdown).


### Devops

#### Configuration

For our cloud-native environments, we deploy [Docker](https://www.docker.com/) containers to [Kubernetes](https://kubernetes.io/) clusters.

For classic environments, we manage the nodes hosting our applications using [Chef](https://www.chef.io/products/chef-infrastructure-management). The provisioning and configuration of each node (where a node is a VM or dedicated server) is defined in our 'chef kitchen' repo, version-tracked and change-managed using git, GitHub and Jira.

##### Evidence
* An application [Dockerfile](samples/Dockerfile).
* A kubernetes application [deployment spec](samples/k8s-deployment.yml).
* [Screenshot](screenshots/lens-ssnext-qa.png) of [Lens](https://k8slens.dev/) IDE showing a running QA staging environment.
* A [node definition](samples/sample_chef_node.json) for a disposable VM used during development for testing deployment configurations.
* A [TLS certificate](screenshots/twg_mpg_certificate.png) installed by [certbot](https://certbot.eff.org/).
* The [Nginx SSL (TLS) config](samples/chef_nginx_ssl.conf.erb).


#### Monitoring

Our live monitoring solution is an in-house tool we call smon. It was built as an aggregator of [collectd](https://collectd.org/) statistics from our nodes, but has come to act as a central board for

* monitoring system health
* monitoring application installations
* tracking software releases through staging environments into production

In addition to smon, we also run [Logwatch](https://sourceforge.net/projects/logwatch/) on each linux node (evidence: [sample output from Logwatch](samples/logwatch.out)). Using collectd to aggregate the same data as Logwatch is on the roadmap for smon.

We additionally have event logging in our Akamai (formerly Linode) infrastructure management console.

##### Evidence
* Here are [some screenshots of smon](screenshots/smon.markdown).
* [Screenshot of Akamai event log](screenshots/akamai_event_log.png)

