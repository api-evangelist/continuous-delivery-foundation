# Continuous Delivery Foundation (continuous-delivery-foundation)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

The Continuous Delivery Foundation (CDF) is a Linux Foundation project that hosts vendor-neutral open source projects for continuous integration, continuous delivery, and DevOps. It is the home of CDEvents, Jenkins, Spinnaker, Screwdriver, Ortelius, JayeX, and was previously the home of Tekton (now a CNCF graduated project) and other CD-focused tooling.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/continuous-delivery-foundation/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags:

 - Automation, CI/CD, DevOps, Linux Foundation, Open Source

## Timestamps

- **Created:** 2026-03-16
- **Modified:** 2026-04-28

## APIs

### CDEvents Specification
CDEvents is a common specification for Continuous Delivery events that enables interoperability across CI/CD systems. It extends the CloudEvents specification and defines event vocabularies for source code control, continuous integration, testing, continuous deployment, continuous operations, and core lifecycle events.

**Human URL:** [https://cdevents.dev/](https://cdevents.dev/)

#### Tags:

 - CDEvents, CI/CD, CloudEvents, Events, Specification

#### Properties

- [Documentation](https://cdevents.dev/docs/)
- [Specification](https://github.com/cdevents/spec)
- [GitHubOrg](https://github.com/cdevents)
- [Community](https://cdevents.dev/community/)

### Jenkins
Jenkins is the leading open source automation server, providing hundreds of plugins for building, deploying, and automating software projects. Jenkins exposes a remote access REST API that supports XML, JSON, and Python representations.

**Human URL:** [https://www.jenkins.io/](https://www.jenkins.io/)

#### Tags:

 - Automation, CI/CD, Jenkins, Pipelines, REST

#### Properties

- [Documentation](https://www.jenkins.io/doc/)
- [APIDocumentation](https://www.jenkins.io/doc/book/using/remote-access-api/)
- [GettingStarted](https://www.jenkins.io/doc/pipeline/tour/getting-started/)
- [GitHubOrg](https://github.com/jenkinsci)

### Spinnaker
Spinnaker is an open-source, multi-cloud continuous delivery platform originally built at Netflix and Google. Spinnaker exposes a Gate REST API that drives pipelines, applications, and deployment workflows across AWS, GCP, Azure, Kubernetes, and other cloud targets.

**Human URL:** [https://spinnaker.io/](https://spinnaker.io/)

#### Tags:

 - CD, Cloud, Deployment, Multi-cloud, Spinnaker

#### Properties

- [Documentation](https://spinnaker.io/docs/)
- [APIDocumentation](https://spinnaker.io/docs/reference/api/)
- [Community](https://spinnaker.io/docs/community/)
- [GitHubOrg](https://github.com/spinnaker)

### Screwdriver
Screwdriver is an open-source build platform designed for Continuous Delivery, originally built at Yahoo. It provides a REST API for managing pipelines, builds, jobs, and webhooks.

**Human URL:** [https://screwdriver.cd/](https://screwdriver.cd/)

#### Tags:

 - Build, CD, CI/CD, Pipelines, Screwdriver

#### Properties

- [Documentation](https://docs.screwdriver.cd/)
- [APIDocumentation](https://docs.screwdriver.cd/api/)
- [GettingStarted](https://docs.screwdriver.cd/user-guide/quickstart)
- [GitHubOrg](https://github.com/screwdriver-cd)

### Ortelius
Ortelius is an open source supply chain evidence store that aggregates continuous security intelligence across the software delivery lifecycle, with APIs for tracking microservice components, SBOMs, vulnerabilities, and deployment evidence.

**Human URL:** [https://ortelius.io/](https://ortelius.io/)

#### Tags:

 - Evidence Store, SBOM, Supply Chain, Security

#### Properties

- [Documentation](https://docs.ortelius.io/)
- [GettingStarted](https://docs.ortelius.io/guides/)
- [GitHubOrg](https://github.com/ortelius)

### JayeX
JayeX is a customizable cloud developer tool suite hosted by the Continuous Delivery Foundation that provides built-in CI/CD capabilities and developer self-service tooling for cloud-native teams.

**Human URL:** [https://jayex.io/](https://jayex.io/)

#### Tags:

 - CI/CD, Cloud Native, Developer Tools, Platform

#### Properties

- [Documentation](https://jayex.io/v3/)
- [Community](https://jayex.io/community/)

### Tekton
Tekton is a Kubernetes-native open source framework for creating CI/CD systems. It defines Custom Resource Definitions for Pipelines, Tasks, PipelineRuns, and TaskRuns. Originally hosted at CDF, Tekton has since moved to the CNCF and is included here for historical context.

**Human URL:** [https://tekton.dev/](https://tekton.dev/)

#### Tags:

 - CI/CD, Kubernetes, Pipelines, Tekton

#### Properties

- [Documentation](https://tekton.dev/docs/)
- [APIDocumentation](https://tekton.dev/docs/pipelines/api/)
- [GitHubOrg](https://github.com/tektoncd)

## Common Properties

- [Website](https://cd.foundation/)
- [Projects](https://cd.foundation/projects/)
- [Documentation](https://cd.foundation/projects/)
- [Blog](https://cd.foundation/blog/)
- [Newsroom](https://cd.foundation/news/)
- [GitHubOrg](https://github.com/cdfoundation)
- [Community](https://cd.foundation/community/)
- [Events](https://cd.foundation/events/)

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
