---
description: "Automatically generated file. DO NOT MODIFY"
---

```javascript

const options = {
	authProvider,
};

const client = Client.init(options);

let res = await client.api('/education/me/assignments/{id}/rubric/$ref')
	.version('beta')
	.delete();

```