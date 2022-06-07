# hitc-zap-demo
Demonstration of Using ZAP as a GitHib Action

## EXERCISE STEPS
In a New Browser Tab, navigate to [https://brokencrystals.com/](https://brokencrystals.com/). This is a website that we will be scanning with a ZAP as triggered by a GitHub action.

In a new browser tab, navigate to [https://github.com/Resistor52/hitc-zap-demo.git](https://github.com/Resistor52/hitc-zap-demo.git) and fork the repository.

Next, click on the "Actions" Tab. Click "New Action" and then "set up a workflow yourself" hyperlink. Name the file `zap.yml` and paste in the following contents in place of the existing content:

```
on:
  push

jobs:
  zap_scan:
    runs-on: ubuntu-latest
    name: Scan brokencrystals Website
    steps:
      - name: ZAP Scan
        uses: zaproxy/action-baseline@v0.3.0
        with:
          target: 'https://brokencrystals.com/'
```

Then click "Start Commit" and then "Commit File"

Notice that our new action is saved as a file in the `.github/workflows/` directory of our repository.

Now, click the actions tab to see the workflow execute.

Click the "Create zap.yml" commit to see the job. Click on the "Scan brokencrystals Website" job to see the details of its execution.

Note that at the bottom of the details, there will be lines that read something like the following:

```
Error: The ZAP Baseline scan has failed, starting to analyze the alerts. err: Error: The process '/usr/bin/docker' failed with exit code 2
Alerts present in the current report: true
Process completed successfully and a new issue #1 has been created for the ZAP Scan.
Total size of all the files uploaded is 21447 bytes
Finished uploading artifact zap_scan. Reported size is 21447 bytes. There were 0 items that failed to upload
```

The reason that the Error message shows is because the ZAP scan found vulnerabilities in the website.

Next, navigate to the "Issues" tab of your Git Repo in the GitHub WebUI. Note that there is a new issue with the scan results. Open the issue and look it over.

At the bottom of the issue is a hyperlink that points to the Scan Report. Clicking that link opens the summary of the workflow and the scan report is at the bottom. Click that link to download it.

When you open the downloaded zipped scan report, you will see that the zip file contains three files:

* report.html
* report_json.json
* report.md

Open one of these files, such as the HTML version, and you will see a nicely formatted report that lists all of the scanner results.

## CONCLUSION

The scan was triggered by the "push" event when we saved the workflow. Remember that there are a number of possible events that can trigger the workflow. These are listed here: [https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)

If this repository contained a web application, the workflow could be designed such that the web application gets loaded into a container and then scanned every time a modification is made to the application source code and pushed to the repo. 
