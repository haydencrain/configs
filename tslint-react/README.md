# TSLint for Typescript React Projects
## Setup - For VS Code
1. Install dependencies:
	- Ensure that typescript has already been setup and installed (example tsconfig.json file can be found within this directory).
	- TSLint:
	`npm install tslint --save`
	- TSLint extensions:
	`npm install tslint-eslint-rules tslint-config-airbnb tslint-react --save`
2. Install the [TSLint for VS Code](https://marketplace.visualstudio.com/items?itemName=eg2.tslint) plugin.
3. Add the TS Lint configuration file (example tslint.json file can be found within this directory). Our current setup is as follows (will most likely change):
```json
{
	"defaultSeverity":  "warning",
	"extends":  [
		"tslint-eslint-rules",
		"tslint-config-airbnb",
		"tslint-react"
	],
	"jsRules":  {},
	"rules":  {
		"ter-indent":  [
			true,
			"tab",
			{  "SwitchCase":  1  }
		],
		"max-line-length":false,  // use ter-max-len instead
		"ter-max-len":  [
			true,
			{
				"code":  120,
				"ignoreImports":  true,
				"ignoreStrings":  true,
				"ignoreTemplateLiterals":  true,
				"ignoreUrls":  true
			}
		],
		"curly":  false,
		"trailing-comma":  false,
		"jsx-boolean-value":  false,
		"jsx-no-multiline-js": false
	},
	"rulesDirectory":  []
}
```
## Enabling Linting of entire project
The TSLint VS Code extension lints an individual file only. If you want to lint your entire workspace or project and want to see the warnings in the `Problems` panel:
1. Add a new npm script that runs tslint:
	`"lint":  "tslint --project tsconfig.json -t verbose"`
2. Define a vscode task that runs the npm script with a problem matcher that extracts the tslint errors into warnings example can be found under .vscode/tasks.json):
	```json
	{
		"version":  "2.0.0",
		"tasks":  [
			{
				"type":  "npm",
				"script":  "lint",
				"problemMatcher":  {
					"base":  "$tslint5",
					"fileLocation":  "absolute"
				}
			}
		],
	}
	```
3. You can now run your task by pressing <kbd>Ctrl</kbd>+<kbd>P</kbd> on Windows (or <kbd>&#8984;</kbd>+<kbd>P</kbd> on Mac), and then typing in `task lint` as demonstrated below:

	![VS Code run task demonstration](https://i.gyazo.com/a0ae4cc705f10147e19c910f00cdc425.gif)

3. Once the linting script has completed, you will see the warnings produced by the npm script in the `Problems` panel and you can navigate to the errors from there. 

### Automatic Linting on Startup
If you would like the task to run automatically on the project startup:
1. Download the [AutoLaunch](https://marketplace.visualstudio.com/items?itemName=philfontaine.autolaunch) VS Code plugin.
2. In your `tasks.json` file, add `"auto": true` to the linting task you created earlier. 

	(**Note:** it is also required for the task to define a `label` in order for AutoLaunch to work)
3. After reloading VS Code, the linting task should run, and the issues will be applied to the `Problems` tab.
