# Runkit-farm-worker
https://orchestrate.io
contact us : <openworkspacesource@gmail.com>
var db = require('orchestrate')(process.env.ORCHESTRATE_KEY)

exports.tonicEndpoint = async function(request, response)
{
    await db.newPatchBuilder("counts", process.env.TONIC_MOUNT_PATH)
            .upsert(true)
            .init("count", 0)
            .inc("count")
            .apply()

    var hits = await db.get("counts", process.env.TONIC_MOUNT_PATH)

    response.end(hits.body.count.toString())
    
}



*We've set our Orchestrate API key in our environment variables, which we use to initialize the db at the start. Then, on every request, we simply increment the count. Finally, we retrieve the current count and return it. 

It's important to do the increment this way, so that concurrent updates aren't lost. We then read back out the current value, which may have been updated by someone else already, but that doesn't much matter. 

The count is stored on a document with an ID matching the "TONIC_MOUNT_PATH" environment variable, which is the URL of this document.



*ll documents on RunKit are public, you can make them searchable by publishing.
all of npm's 1,000,000+ packages are already pre-installed, just require() them
use arrow functions, classes, template strings, and most of ES6. See everything we support here.
await any promise instead of using callbacks (example)
store secrets in environment variables
export an endpoint function to turn any notebook into an API (endpoint docs)

