# Status vs Checks API
> Compare Status API with Checks API

### Status
The [Status API](https://developer.github.com/v3/repos/statuses/#list-statuses-for-a-specific-ref) allows external services to **mark commits with a `state`**, which is then reflected in pull requests involving those commits. For example: [`pull/1`](https://github.com/imjohnbo/status-vs-checks-api/pull/1).

<details><summary>API request</summary>

```
curl --request POST \
  --url https://api.github.com/repos/imjohnbo/status-vs-checks-api/statuses/0bd9d12e3f92cb554621084faf0bf30b931ba8a9 \
  --header 'authorization: Bearer <token>' \
  --header 'content-type: application/json' \
  --data '{
  "state": "success",
  "target_url": "https://example.com/build/status",
  "description": "The build succeeded!",
  "context": "ci"
}
```
</details>

![status](https://user-images.githubusercontent.com/2993937/83883955-334aa000-a712-11ea-8c3f-76dddbce9704.gif)

### Checks
The more robust [Checks API](https://developer.github.com/v3/checks/) is only available to GitHub Apps and allows for **reporting rich statuses, annotate lines of code with detailed information, and kick off reruns**. For example: [`pull/2`](https://github.com/imjohnbo/status-vs-checks-api/pull/2), [`pull/2/checks`](https://github.com/imjohnbo/status-vs-checks-api/pull/2/checks).


<details><summary>API request</summary>

```
curl --request POST \
  --url https://api.github.com/repos/imjohnbo/status-vs-checks-api/check-runs \
  --header 'accept: application/vnd.github.antiope-preview+json' \
  --header 'authorization: Bearer <token>' \
  --header 'content-type: application/json' \
  --data '{
	"name": "name",
	"head_sha": "904f3da45f338f777bff682434afc675a63952cf",
	"details_url": "http://example.com/details_url",
	"external_id": "external_id_12345",
	"status": "completed",
	"started_at": "2020-06-05T12:00:00Z",
	"conclusion": "success",
	"completed_at": "2020-06-05T12:05:00Z",
	"output": {
		"title": "Run was successful",
		"summary": "Summary of **check** ~run~",
		"text": "Text about check run",
		"annotations": [
			{
				"path": "README.md",
				"start_line": 5,
				"end_line": 5,
				"annotation_level": "notice",
				"message": "Head'\''s up!",
				"title": "We noticed something",
				"raw_details": "Details details details"
			}
		],
		"images": [
			{
				"alt": "Terraform dependency graph",
				"image_url": "https://www.terraform.io/assets/images/docs/graph-example-8a4f085e.png",
				"caption": "Terraform dependency graph"
			}
		]
	},
	"actions": [
		{
			"label": "Fix this",
			"description": "Automatically fix this",
			"identifier": "12345"
		}
	]
}
```
</details>

![checks](https://user-images.githubusercontent.com/2993937/83884694-4447e100-a713-11ea-8a08-2ea4d776f3d3.gif)

## Resources

- [Announcement](https://github.blog/2018-05-07-introducing-checks-api/)
- [Creating CI tests with the Checks API](https://developer.github.com/apps/quickstart-guides/creating-ci-tests-with-the-checks-api)