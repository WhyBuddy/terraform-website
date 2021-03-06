---
layout: "cloud"
page_title: "Policy Checks - API Docs - Terraform Cloud"
---

[200]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200
[201]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/201
[202]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/202
[204]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/204
[400]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/400
[401]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/401
[403]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/403
[404]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404
[409]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/409
[412]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/412
[422]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422
[429]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429
[500]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500
[504]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/504
[JSON API document]: /docs/cloud/api/index.html#json-api-documents
[JSON API error object]: http://jsonapi.org/format/#error-objects

# Policy Checks API

## List policy checks

This endpoint lists the policy checks in a run.

-> **Note**: The `sentinel` hash in the `result` attribute structure represents low-level Sentinel details generated by the policy engine. The keys or structure may change over time. Use the data in this hash at your own risk.

| Method | Path           |
| :----- | :------------- |
| GET | /runs/:run_id/policy-checks |

### Parameters

- `run_id` (`string: <required>`) - specifies the run ID for which to list policy checks

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $TOKEN" \
  https://app.terraform.io/api/v2/runs/run-CZcmD7eagjhyXavN/policy-checks
```

### Sample Response

```json
{
  "data": [
    {
      "id": "polchk-9VYRc9bpfJEsnwum",
      "type": "policy-checks",
      "attributes": {
        "result": {
          "result": false,
          "passed": 0,
          "total-failed": 1,
          "hard-failed": 0,
          "soft-failed": 1,
          "advisory-failed": 0,
          "duration-ms": 0,
          "sentinel": {...}
        },
        "scope": "organization",
        "status": "soft_failed",
        "status-timestamps": {
          "queued-at": "2017-11-29T20:02:17+00:00",
          "soft-failed-at": "2017-11-29T20:02:20+00:00"
        },
        "actions": {
          "is-overridable": true
        },
        "permissions": {
          "can-override": false
        }
      },
      "links": {
        "output": "/api/v2/policy-checks/polchk-9VYRc9bpfJEsnwum/output"
      }
    }
  ]
}
```

## Override Policy

This endpoint overrides a soft-mandatory or warning policy.

-> **Note**: The `sentinel` hash in the `result` attribute structure represents low-level Sentinel details generated by the policy engine. The keys or structure may change over time. Use the data in this hash at your own risk.

| Method | Path           |
| :----- | :------------- |
| POST | /policy-checks/:policy_check_id/actions/override |

### Parameters

- `policy_check_id` (`string: <required>`) - specifies the ID for the policy check to override

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request POST \
  https://app.terraform.io/api/v2/policy-checks/polchk-EasPB4Srx5NAiWAU/actions/override
```

### Sample Response

```json
{
  "data": {
    "id": "polchk-EasPB4Srx5NAiWAU",
    "type": "policy-checks",
    "attributes": {
      "result": {
        "result": false,
        "passed": 0,
        "total-failed": 1,
        "hard-failed": 0,
        "soft-failed": 1,
        "advisory-failed": 0,
        "duration-ms": 0,
        "sentinel": {...}
      },
      "scope": "organization",
      "status": "overridden",
      "status-timestamps": {
        "queued-at": "2017-11-29T20:13:37+00:00",
        "soft-failed-at": "2017-11-29T20:13:40+00:00",
        "overridden-at": "2017-11-29T20:14:11+00:00"
      },
      "actions": {
        "is-overridable": true
      },
      "permissions": {
        "can-override": false
      }
    },
    "links": {
      "output": "/api/v2/policy-checks/polchk-EasPB4Srx5NAiWAU/output"
    }
  }
}
```
