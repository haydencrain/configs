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
		"curly":  [true,  "as-needed"],
		"trailing-comma":  false,
		"jsx-boolean-value":  false
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
				"label":  "build",
				"command":  "dotnet",
				"type":  "process",
				"args":  [
					"build",
					"${workspaceFolder}/Sebir.csproj"
				],
				"problemMatcher":  "$msCompile"
			},
			{
				"type":  "npm",
				"script":  "lint",
				"problemMatcher":  {
					"base":  "$tslint5",
					"fileLocation":  "relative"
				}
			}
		],
	}
	```
3. When you then run the `npm run lint` task you will see the warnings produced by the npm script in the `Problems` panel and you can navigate to the errors from there. 
	
	If you are running a .Net Core application, it would be useful to set up the script to run automatically during the Build process. This can be achieved by adding the following to the `.csproj` file:
	```xml
	<Target  Name="NpmLint"  BeforeTargets="Build">
		<Exec  Command="npm run lint" />
	</Target>
	```