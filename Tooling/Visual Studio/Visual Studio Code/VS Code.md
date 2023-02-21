# Visual Studio Code

## Create folder shortcut

1. First create vs code shortcut

2. Right click choose shortcut pane

3. ***Append*** folder path to target field, remember to add space between them

4. Choose icon. [[Basic]]


## Python Extension

##### Tips:
- Add extra path

	``` json
	{
	 "python.analysis.extraPaths": ["./sources"]
	}
	```
- Ignore type checking: add comment at the end of line, after code
  
  ```
  # type: ignore
  ```