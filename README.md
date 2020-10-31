# GHPR: The dataset of GitHub PRs and issues
GHPR contains data about pull requests that have fixed one or more issues on GitHub.

The dataset can be found at [`/ghpr.csv`](./ghpr.csv).
A small sample of the dataset is also available at [`/ghpr-sample.csv`](./ghpr-sample.csv).

## Structure
Each instance of GHPR contains data about an issue and a pull request, where the pull request has fixed the issue.

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

Rows of the dataset are sorted by repository owner username, repository name, and then pull request number.

## Method
The data is collected using the [GitHub REST API](https://docs.github.com/en/free-pro-team@latest/rest).
The data collection flow is as follows:
- For repository *R*:
  - For each merged pull request *P* in *R*:
    - If *P* links at least one issue in *R* using a [GitHub keyword](https://docs.github.com/en/free-pro-team@latest/github/managing-your-work-on-github/linking-a-pull-request-to-an-issue#linking-a-pull-request-to-an-issue-using-a-keyword):
      - For each linked issue *I*:
        - *(I, P)* is a member of GHPR.

The dataset is created using [GHPR Tools](https://github.com/soroushj/ghpr-tools).

The [raw data](https://github.com/soroushj/ghpr-dataset-raw) for this dataset is also available,
where you can find a JSON file for each issue and pull request present in the dataset.

## Data
The current version of GHPR contains 14,384 rows.
The data is collected from [CNCF graduated projects](https://www.cncf.io/projects/), specifically, the following repositories:
- [`containerd/containerd`](https://github.com/containerd/containerd)
- [`coredns/coredns`](https://github.com/coredns/coredns)
- [`envoyproxy/envoy`](https://github.com/envoyproxy/envoy)
- [`fluent/fluentd`](https://github.com/fluent/fluentd)
- [`goharbor/harbor`](https://github.com/goharbor/harbor)
- [`helm/helm`](https://github.com/helm/helm)
- [`jaegertracing/jaeger`](https://github.com/jaegertracing/jaeger)
- [`kubernetes/kubernetes`](https://github.com/kubernetes/kubernetes)
- [`prometheus/prometheus`](https://github.com/prometheus/prometheus)
- [`rook/rook`](https://github.com/rook/rook)
- [`theupdateframework/specification`](https://github.com/theupdateframework/specification)
- [`tikv/tikv`](https://github.com/tikv/tikv)
- [`vitessio/vitess`](https://github.com/vitessio/vitess)

Dataset last updated: 2020-10-28.
