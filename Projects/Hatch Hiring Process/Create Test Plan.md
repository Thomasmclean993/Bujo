# Test Cases

## Schema & Validation Test
#### Message Schema Validation 
- [ ] Valid Type
	- Input: Proper Payload with `type: "sms"`, valid fields.
	- Expected: Changeset to be valid
- [ ] Invalid Type
	- Input: Invalid `type:fax`
	- expected: Changeset invalid, appropriate
#### Missing Required Fields 
- [ ] Omit `from` or `to` 
	- Expected: Validation failures, appriorate error message
#### Attachment field normalization
- [ ] Input: `attachments: null`
- Expected: Normalized to empty list `[]`
#### Conversation schema Validation 
- [ ] Input: paricipants list vlaid, stored correctly as json/array
	- Expected: Valid changeset

## 