# SQLMap Output Descriptio


## Log Messages Description
> The following are some of the most common messages usually found during a scan of SQLMap, along with an example of each from the previous exercise and its description.

### URL content is stable

Log Message:

- "target URL content is stable"
>This means that there are no major changes between responses in case of continuous identical requests.

### Parameter appears to be dynamic
Log Message:

- "GET parameter 'id' appears to be dynamic"
>It is always desired for the tested parameter to be "dynamic," as it is a sign that any changes made to its value would result in a change in the response; hence the parameter may be linked to a database. 