# GHPR: The dataset of GitHub PRs and issues
GHPR contains data about pull requests that have fixed one or more issues on GitHub.

The dataset can be found at [`/ghpr.csv`](./ghpr.csv).
A small sample of the dataset is also available at [`/ghpr-sample.csv`](./ghpr-sample.csv).

## Structure
Each instance of GHPR contains data about an issue and a pull request, where the pull request has fixed the issue.
Note that in some cases, a single pull request is linked to multiple issues or vice versa.

The dataset is a CSV file with the following columns:
- `repo_id` - Integer
- `issue_number` - Integer
- `issue_title` - Text
- `issue_body_md` - Text, in Markdown format, can be empty
- `issue_body_plain` - Text, in plain text, can be empty
- `issue_created_at` - Integer, in Unix time
- `issue_author_id` - Integer
- `issue_author_association` - Integer enum (see values below)
- `issue_label_ids` - Comma-separated integers, can be empty
- `pull_number` - Integer
- `pull_created_at` - Integer, in Unix time
- `pull_merged_at` - Integer, in Unix time
- `pull_comments` - Integer
- `pull_review_comments` - Integer
- `pull_commits` - Integer
- `pull_additions` - Integer
- `pull_deletions` - Integer
- `pull_changed_files` - Integer

The value of `issue_body_plain` is converted from `issue_body_md`.
The conversion is not always perfect.
In some cases, `issue_body_plain` still contains some Markdown tags.

The value of `issue_author_association` can be one of the following:
- `0` - Collaborator
- `1` - Contributor
- `2` - First-timer
- `3` - First-time contributor
- `4` - Mannequin
- `5` - Member
- `6` - None
- `7` - Owner

See [GitHub docs](https://docs.github.com/en/free-pro-team@latest/graphql/reference/enums#commentauthorassociation) for more details on author association.

Rows of the dataset are sorted by repository owner username, repository name, pull request number, and then issue number.

## Method
The data is collected using the [GitHub REST API](https://docs.github.com/en/free-pro-team@latest/rest).
The data collection flow is as follows:
- For repository *R*:
  - For each merged pull request *P* in *R*:
    - For each issue *I* that is linked by *P* using a [GitHub keyword](https://docs.github.com/en/free-pro-team@latest/github/managing-your-work-on-github/linking-a-pull-request-to-an-issue#linking-a-pull-request-to-an-issue-using-a-keyword) and is in *R*:
      - *(I, P)* is a member of GHPR.

The dataset is created using [GHPR Tools](https://github.com/soroushj/ghpr-tools).

The [raw data](https://github.com/soroushj/ghpr-dataset-raw) for this dataset is also available,
where you can find a JSON file for each issue and pull request present in the dataset.

## Data
This version of GHPR contains 14,384 instances.
The data is collected in October 2020 from [CNCF graduated projects](https://www.cncf.io/projects/), specifically, the following repositories:
| Repository                                                                                | # instances |
| ----------------------------------------------------------------------------------------- | ----------- |
| [`containerd/containerd`](https://github.com/containerd/containerd)                       | 351         |
| [`coredns/coredns`](https://github.com/coredns/coredns)                                   | 227         |
| [`envoyproxy/envoy`](https://github.com/envoyproxy/envoy)                                 | 1,229       |
| [`fluent/fluentd`](https://github.com/fluent/fluentd)                                     | 161         |
| [`goharbor/harbor`](https://github.com/goharbor/harbor)                                   | 598         |
| [`helm/helm`](https://github.com/helm/helm)                                               | 859         |
| [`jaegertracing/jaeger`](https://github.com/jaegertracing/jaeger)                         | 455         |
| [`kubernetes/kubernetes`](https://github.com/kubernetes/kubernetes)                       | 8,323       |
| [`prometheus/prometheus`](https://github.com/prometheus/prometheus)                       | 543         |
| [`rook/rook`](https://github.com/rook/rook)                                               | 946         |
| [`theupdateframework/specification`](https://github.com/theupdateframework/specification) | 13          |
| [`tikv/tikv`](https://github.com/tikv/tikv)                                               | 469         |
| [`vitessio/vitess`](https://github.com/vitessio/vitess)                                   | 210         |
