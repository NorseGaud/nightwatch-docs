## Parallel Running

Nightwatch supports running the tests in parallel in two main ways: 
- via test workers
- by running multiple test environments in parallel

#### Via Workers

When this is enabled the test runner will launch a configurable number of child processes and then distribute the loaded tests over to be ran in parallel.

To enable test workers, set the `test_workers` top-level property, like so:

<pre><code class="language-javascript">{
  "test_workers": {
    "enabled": true,
    "workers": "auto"
  }
}
</code></pre>

or:

<pre><code class="language-javascript">{"test_workers": true}</code></pre>

<br>

The `workers` option configures how many child processes can run concurrently.

- `"auto"` - determined by number of CPUs e.g. 4 CPUs means 4 workers
- `{number}` - specifies an exact number of workers

<br>
Another way is to pass the `--parallel` cli switch:
<pre><code class="language-bash">nightwatch --parallel</code></pre>

Test concurrency is done at the file level. Each test file will fill a test worker slot. Individual tests/steps in a test file will not run concurrently.

<div class="alert alert-warning">
To improve support for displaying the output when running tests in parallel, we recommend setting `detailed_output` to `false` in your test settings (and also make sure `live_output` is enabled.
</div>

#### Multiple Environments

<pre><code class="language-bash">nightwatch -e firefox,chrome</code></pre>

The above will run two environments named `firefox` and `chrome` in parallel.

#### Terminal Output

Each environment will be run as a separate [`child_process`](https://nodejs.org/api/child_process.html) and the output will be sent to the main process.

To make the output easier to read, Nightwatch by default buffers the output from each child process and displays everything at the end, grouped by environment.

<div class="alert alert-warning">
  If you'd like to disable the output buffering and see the output from each child process as it is sent to stdout, simply set the property <code>"live_output" : true</code> on the top level in your <code>nightwatch.json</code> (e.g. after <code>selenium</code>).
</div>

<div class="alert alert-info">
  You can create a separate environment per browser (by chaining <code>desiredCapabilities</code>) and then run them in parallel. In addition, using the <code>filter</code> and <code>exclude</code> options tests can be split per environment in order to be ran in parallel.
</div>

#### Via Workers + Multiple Environments

It is very useful to be able to run your tests against multiple browsers in parallel and also distribute your testcases across multiple workers.
From **v1.7** you are able to do just that.
<pre><code class="language-bash">nightwatch -e firefox,chrome --parallel</code></pre>

The above will run two environments named `firefox` and `chrome` in parallel.

- Previous: [Using test tags](/guide/running-tests/test-tags.html)
- Next: [Disabling or skipping Tests](/guide/running-tests/disabling-tests.html)
