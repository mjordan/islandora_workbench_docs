You can define prompts to the user by adding settings to your configuration file. There are two types: 1) a prompt to ask the user whether they have run `--check`, and 2) configurable prompts that allow you to define your own "y/n" questions to the user.

The "Have you run --check? (y/n)" prompt simply displays that question to the end user and asks for a "y" or "n" response. "y" resumes normal operation, "n" (or any other response) causes Workbench to exit. To show the user this prompt, add the following to your config file:

`remind_user_to_run_check: true`

If this configuration setting is present, the user will be prompted to answer "Have you run --check? (y/n)" when Workbench is run without the `--check` argumement. (This prompt is separate from the next type of prompt because it contains special logic such that it is never presented to the user in `--check` mode.)

The second type of prompt, the configurable prompts, allow the creation of custom "y/n" questions that the user must answer "y" to in order to proceed. These prompts are shown to the user regardless of whether they are running Workbench with the `--check` argument. A situation where you may want to ask the user a question of this sort is when creating media that are normally created by Islandora as derivatives. In that case, it is usefult to remind the user that they need to temporarily disable the Contexts that generate the derivatives.

These "y/n" questions you want to prompt users with are listed within the `user_prompts` config setting like this:

```
user_prompts:
  - Have you inspected your Workbench log to look for issues you can fix? (y/n)
  - If you are adding "additional files", have you temporarily disabled the Contexts that would generate corresponding derivatives? (y/n)
```

Each prompt is shown to the user in the order they are listed, each asking for a "y/n" response. If the users responds by entering "y", the next prompt is shown. If they respond with "n" or any other key, Workbench logs the prompt and response, and exits. Note that you must include the "(y/n)" text in your prompts; Workbench doesn't add it automatically.

A couple notes:

* The user must hit Enter after entering "y" or "n".
* You can include both the `remind_user_to_run_check` setting and the `user_prompts` settings in your config file. If you include both, "Have you run --check? (y/n)" is presented to the user first, then the customizable prompts.
* If the user responds  "n" to any prompt (or any response other than "y"), the prompt along with their response is logged before Workbench exits.
* Prompts can be overridden with an implied "y" answer by including the `--skip_user_prompts` argument when running Workbench. This is useful if your config file includes either the `remind_user_to_run_check` or `user_prompts` during testing or scripted use of Workbench.

