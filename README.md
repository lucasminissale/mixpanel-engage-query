Command-line tool to query the [Mixpanel Engage API](https://mixpanel.com/docs/api-documentation/data-export-api#engage-default) for People Data. With other words, export list of Mixpanel users with selected properties, optionally filtered by query on property values.

## Installation

Clone the repository:
``git clone https://github.com/stpe/mixpanel-engage-query.git``

Install [Node.js](http://nodejs.org/).

Run ``npm install`` in the directory. That's it!

## Example

#### Get help

```
$ node engage.js
Usage: node ./engage.js -k [string] -s [string]

Options:
  -k, --key         Mixpanel API key                                                  [required]
  -s, --secret      Mixpanel API secret                                               [required]
  -q, --query       A segmentation expression
  -f, --format      Output format, json or csv                                        [default: "json"]
  -p, --properties  Properties to output. Outputs all properties if none specified.
  -r, --required    Skip entries where the required properties are not set (e.g. '$email $first_name').
  -h, --help

Missing required arguments: k, s
```

Note that the Mixpanel API key and secret may also be set using environment variables `MIXPANEL_API_KEY` and `MIXPANEL_API_SECRET` or in a [.env](https://github.com/motdotla/dotenv) file.

#### Get everything

``node engage.js -k MIXPANEL_API_KEY -s MIXPANEL_API_SECRET``

Example output:
```
{
    "$browser": "Chrome",
    "$city": "Kathmandu",
    "$country_code": "NP",
    "$initial_referrer": "$direct",
    "$initial_referring_domain": "$direct",
    "$os": "Windows",
    "$timezone": "Asia/Katmandu",
    "id": "279267",
    "nickname": "bamigasectorone",
    "$last_seen": "2015-04-15T13:07:30",
    "$distinct_id": "15b9cba739b75-03c7e24a3-459c0418-101270-13d9bfa739ca6"
}
```

Note that `$distinct_id` is included as a property for convenience.

#### Only output specific fields

``node engage.js -k MIXPANEL_API_KEY -s MIXPANEL_API_SECRET -p '$email $first_name'``

Example output:
```
{ '$email': 'jocke@bigcompany.se', '$first_name': 'Joakim' }
{ '$email': 'henrik@gmail.com', '$first_name': 'Henrik' }
{ '$email': 'theguy@gmail.com', '$first_name': 'Jonas' }
```

Note: one JSON entry per row.

#### Output as CVS instead of JSON

``node engage.js -k MIXPANEL_API_KEY -s MIXPANEL_API_SECRET -f 'csv'``

Example output:
```
jocke@bigcompany.se;Joakim
henrik@gmail.com;Henrik
theguy@gmail.com;Jonas
```

Note: currently no special escaping or similar is implemented, so depending on values csv may end up invalid.

#### Query using expression

This example returns people with $last_seen timestamp greater (later) than 24th of April (see the Mixpanel documentation for [segmentation expressions](https://mixpanel.com/docs/api-documentation/data-export-api#segmentation-expressions)).

``node engage.js -k MIXPANEL_API_KEY -s MIXPANEL_API_SECRET -q 'properties["$last_seen"] > "2015-04-24T23:00:00"'``

