# Diagnosing failing GitHub Actions

If a repo has GHA workflows, there are a variety of triggers but generally the problems you encounter will be on pull requests.

## Quick overview of CI/CD

Continuous integration and continuous deployment/devlivery (CI/CD) is the practice of automating testing on every code change (CI) and automating deployment of services or packages (CD). In practice this means whenever you open a PR on a repo with CI/CD workflows defined, it will automatically run tests, linters, whatever other checks to make sure the code additions don't break anything and are up to coding standards. For scientific software packages, the burden of CD is generally limited to pushing packages to package repos (PyPi, conda, npm, etc) because we don't have many deployed services (think web services or apps). Typically these CD workflows will be triggered on merging code into the main branch.

## Quick overview of GHAs

GitHub Actions (GHAs) is GitHub's built in CI/CD service. It provides GHA runners (machines with a Linux OS, 7GB mem, and a decent CPU by default, all those things can be upgraded) for free (!!! (up to some limit that's surprisingly high (for open source projects)). The workflow definitions for GHAs can be found in `.github/workflows/` as yaml files and will usually define name, on (which specifies triggers for the workflow), and jobs (which define the actual steps of the workflow).

There are other (also with generous free tiers for open source) CI/CD platforms like CircleCI, TravisCI, etc. Explore as you will.

## How to find output from GHAs

You can see all GHA runs on the Actions page (https://github.com/org/repo/actions/) or you can see the latest runs for a PR on that PR's page (https://github.com/org/repo/pull/1). From the PR page, click on Details next to a workflow to see its output. From the Actions page, click on any run and then click on the job you want to see output for (usually there will be only one). The output will be broken down by each step defined in the workflow and you should be able to expand each section individually.

## Failing tests

Go to the workflow output and check which step is failing. Usually it will be the Run tests step (or something similarly named). Expand and scroll to the bottom for a summary of what went wrong, then scroll back up to search for more specifics (they can get kinda buried sometimes).

## Failing linters

Run formatter on code and commit the results to see if that fixes it. For python, use [black](https://pypi.org/project/black/) (e.g. `black src/ tests/`). For snakemake, use [snakefmt](https://anaconda.org/bioconda/snakefmt) (e.g. `snakefmt *.smk`). For R, use [styler](https://www.tidyverse.org/blog/2017/12/styler-1.0.0/) (e.g. `styler::style_dir()`).

Note: All these choices of linting/formatting software are half opinion, half trying to stay in line with the CI tools we use (which are also chosen as a matter of opinion). If there's reason to use others, use others.

If the linters continue to fail, you can check the output to make sure you actually included the files it's complaining about in your restyling command. If it's complaining about something that you did run the styler on, you can try to update the version of the styler or see if there are weird defaults set or just give up. It's not the end of the world if the linter fails.

## Failing CodeCov

CodeCov is a 3rd party tool we sometimes use for providing a dashboard and stats on test coverage. Test coverage is a ratio of how many lines of code are hit while running all tests to how many lines there are total. It's usually a good primary indicator of how thorough your testing suite is. If it's failing, usually it's because tests are failing. Or it could be something else.

## Failing Codacy

Codacy is another 3rd party tool that performs static analysis on the codebase. Static analysis is great for catching bad formatting (it includes linters) and dangerous code patterns. If your PR is flagged by Codacy for introducing new issues, you can follow the link to their site to view them and potential solutions. Like with the linter, fix what's easy to fix but if something is a real pain or has reason not to be fixed, it's not a terrible thing to have a few issues hanging around in Codacy. You can also turn checks on and off in Codacy if it's bugging you.
