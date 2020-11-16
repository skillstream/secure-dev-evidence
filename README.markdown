Secure Development Evidence Pack
--------------------------------

This resource shares some general information about Skillstream's software development lifecycle, with particular reference to our security practices, and links to evidence for anyone undertaking due diligence.

The lifecycle is backed by a variant of the ['git-flow'](https://nvie.com/posts/a-successful-git-branching-model/) branching strategy, with [Atlassian Jira](https://www.atlassian.com/software/jira) tracking features and issues and [GitHub](https://github.com/) supporting peer review, test automation and release tracking.


### List of Tools

#### Day-to-day
Below is a list of tools we use during development to assist us, backed by [Github Actions](https://github.com/features/actions) as laid-out in this [sample workflow](github-workflow-ruby.yml).

* [bundler-audit](https://github.com/rubysec/bundler-audit#readme) checks 3rd-party libraries against the CVE database for known vulnerabilities in Ruby gems.
  - Evidence: [sample output from bundle audit](bundle-audit.out)
* [yarn-audit](https://classic.yarnpkg.com/en/docs/cli/audit/) checks 3rd-party libraries against the CVE database for known vulnerabilities in nvm packages.
  - Evidence: [sample output from yarn audit](yarn-audit.out)
* [Brakeman](https://brakemanscanner.org/) analyses projects which use the Ruby on Rails framework for common vulnerabilities.
  - Brief outline of how Brakeman helps test against some of the OWASP Top 10, from [Stack Overflow](https://stackoverflow.com/a/35229152/418602)
  - Evidence: [sample output from Brakeman](brakeman.out)
* [Rubocop](https://rubocop.org/) enforces in-house coding style and is also good at highlighting the subset of bugs which can arise from violating conventions, e.g. variable shadowing, method duplication after a merge, etc.
  - Evidence: [sample output from Rubocop](rubocop.out)
* [RSpec](https://rspec.info/) is used to define expected behaviour and test those expectations. Some of those expectations explicitly cover access control, but other tests will prevent the many bugs that could also have had security implications.
  - Evidence: [sample output from RSpec](rspec.out)
  - Supporting tools:
    * [FactoryBot](https://github.com/thoughtbot/factory_bot) for churning-out randomised test objects
    * [Faker](https://github.com/faker-ruby/faker) for generating realistic test data
    * [Capybara](http://teamcapybara.github.io/capybara/) for front-end integration testing



#### Penetration testing
These are the main tools we use for periodic review of application security:

* [OWASP ZAP](https://www.zaproxy.org/) to scan our web apps for vulnerabilities.
* [Kali Linux VM](https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/) because it comes pre-installed with ZAP and a lot of other useful tools, and is a safe environment for handling malware if required.

##### Evidence
Here are some screenshots of ZAP in use:

![Automated scan](screenshots/automated_scan.png)

![Spider](screenshots/spider.png)

![Active scan](screenshots/active_scan.png)
