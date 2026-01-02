# Debug Mode

{% embed url="https://youtu.be/UVTh8af8uy4?si=f31WUcosDWif3nmY" %}

### Purpose

Debug mode is designed to:

* Investigate failing behavior
* Trace root causes
* Reduce time spent on guesswork
* Assist, not replace, debugging logic

> This mode is about **understanding**, not generating code.

### What Debug Mode Is Good At

* Explaining stack traces
* Identifying likely failure points
* Highlighting logical inconsistencies
* Suggesting diagnostic steps
* Mapping symptoms to root causes

It helps you **reason faster**, not think less.

### Correct Usage

Use Debug mode when:

* A test is failing unexpectedly
* A bug is reproducible but unclear
* Logs do not immediately explain behavior
* You need a second set of eyes on the logic flow

Typical workflow:

1. Reproduce the issue
2. Select relevant files or error output
3. Ask Cursor to analyze the failure
4. Validate its reasoning
5. Apply fixes manually

### Wrong Usage

* Asking Debug mode to “just fix it.”
* Accepting suggested patches without understanding
* Debugging without first reproducing the issue
* Using it as a substitute for logs or breakpoints

If you skip reasoning, you learn nothing.

### Key Rule

> Debug mode accelerates diagnosis.\
> It does not replace debugging skills.

If you cannot explain the bug after using Debug mode, you misused it.

{% columns %}
{% column %}
{% embed url="https://www.youtube.com/shorts/Ez_Lm-9VkvQ?feature=share" %}
{% endcolumn %}

{% column %}
{% embed url="https://youtube.com/shorts/aZivENmq8pY?si=oVXztpra2sYyRrGO" %}
{% endcolumn %}
{% endcolumns %}
