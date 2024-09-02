You can define prompts to the user within your configuration file. There are two types: 1) a prompt to ask the user whether they have run `--check`, and 2) configurable prompts that allow you to define your own "y/n" questions to the user.

The "Have you run --check? (y/n)" prompt simply displays that question to the end user and asks for a "y" or "n" response. "y" resumes normal operation, "n" (or any other response) causes Workbench to exit. To configure this prompt, add the following to your config file:

`remind_user_to_run_check: true`

If this configuration setting is present, the user will be prompted to answer "Have you run --check? (y/n)" when Workbench is run without the `--check` argumement.

The second type of prompt, the configurable prompts, allow the creation of custom "y/n" questions that the user must answer "y" to in order to proceed. These prompts are shown  to the user regardless of whether they are running Workbench with the `--check` argument. A situation where you may want to ask the user a question of this sort is when creating media that are normally created by Islandora as derivatives. In that case, the Contexts that generate the derivatives should be temporarily disabled.

These "y/n" questions you want to prompt users with are listed within the `user_prompts` config setting like this:

```
user_prompts:
  - Have you prepared your "additional files"? (y/n)
  - Have you temporarily disabled the applicable Contexts? (y/n)
```

Each one is shown to the user in the order they are listed, each asking for a "y/n" response. If the users responds by entering "y", the next prompt is shown. If they respond with "n" or any other key, Workbench logs the prompt and exits.


A couple notes:

* The user must hit Enter after entering "y" or "n".
* The "Have you run --check?" prompt is completely independent of the configurable prompts; it's the only built-in prompt Workbench provides; it contains special logic such that it is never presented to the user in `--check` mode.
* You can include both the `remind_user_to_run_check` setting and the `user_prompts` setting without them conflicting with each other. If you include both, "Have you run --check? (y/n)" is shown first, then the customizable prompts.
* If the user responds  "n" to any prompt (or any response other than "y"), thep prompt along with their response is logged before Workbench exits.
* Prompts can be overridden with an implied "y" answer by including the `--skip_user_prompts` argument when running Workbench. This is useful if your config file includes either the `remind_user_to_run_check` or `user_prompts` during testing or scripted use of Workbench.

