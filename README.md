# Roblox-Plugin-Projects
VERSION: v1.0

SET UP:
	- Move 'Design' folder to 'ReplicatedStorage'
	- Move 'Menu' ScreenGui to 'StarterGui'
	- Press F5 to check if everythis is working

DOCUMENTATION:
	- In order to controll menu you have to 'require' it
	- Use require(%path%.Controller)
	- You have to controll menu UI 'visibility' manually!
	- 'Controller' provides basic and advanced controll of menu background animation
	- Atributes are in module script - Controller
	- In order to change style you have to ENABLE next gen studion and UI styling
		- File>Beta Features>Next Gen Studio Preview
		- File> BetaFeatures>UI styling
	
	ATTRIBUTES:
		- 'AnimUpdateTick' - time in seconds
						   - defines interval between calling a fuction to change background cells character
		- 'FadeUpdateTick' - time in seconds
						   - defines interval between changing a cell in functions Background.Fade and Background.FadeRandom
		- 'BackgroundCharacters' - Characters that should be displayed in cells
		- 'MaxHorizontalCells' - Number of cells in Y axis
		- 'TitleText' - Title of menu (You dont have to fill this)
		- 'TitleTopOffset' - Title offset from top of the screen
	
	ENUMERATORS:
		- SetMode ------ show
		|         ------ clear
		- NumberModes -- number
		|              - percentage
		- Direction ---- random
					---- horizontal
					---- vertical
	
	VARIABLES:
		- Variables.XCells - width of menu in cells
		- Variables.YCells - height of menu in cells
	
	FUNCTIONS:
		
		NewButton(pos: Udmi2, text: string, anchor: number)
			- Create a new button in menu with specific location
			- 'anchor' defines horizontal anchor of text
			USE:
				local module = require(%path%.Controller)
				local settings_button = module.NewButton(Udmi2.new(0, 0.5, 0, 0.5), "Settings", 0.5) --create a new button in the middle of screen
				
				settings_button.onClick.Event:Connect(function() --hadle button click event
					--do staff here
				end)
		
		Background.Fade(fragment: number, uniqe: BoolValue, set_mode: SetMode)
			- 'fragment' - fraction of all cells in menu background that will be changed based on 'set_mode' per FadeUpdateTick
						 - number between 0 and 1
			- 'uniqe' - defines if cell with tag 'uniqe' should be changed based on 'set_mode'
			          - if it's false these cells are ignored
			- 'set_mode' - setting for each cell if it will be visible or not
			             - 'show' - visible
			             - 'clear' - not visible
			             
			- This function will affect ALL cells based on 'uniqe' argument
			- Position of each cell that should be changed is picked randomly (in next update will be added new argument 'direction')
			USE:
				local module = require(%path%.Controller)
				module.Background.Fade(0.2, false, module.Enumerators.SetMode.clear) --hide 20% of all cells every FadeUpdateTick
		
		Background.FadeRangom(chance: number, uniqe: boolean, set_mode: SetMode)
			- 'chance' - number between 0 and 1
					   - defines chance for changing each cell based on 'set_mode' setting per FadeUpdateTick
			- 'uniqe' - defines if cell with tag 'uniqe' should be changed based on 'set_mode'
			          - if it's false these cells are ignored
			- 'set_mode' - setting for each cell if it will be visible or not
			             - 'show' - visible
			             - 'clear' - not visible
			             
			- This function will affect ALL cells based on 'uniqe' argument
			USE:
				local module = require(%path%.Controller)
				module.Background.FadeRandom(0.2, false, module.Enumerators.SetMode.clear) --each cell every FadeUpdateTick has a chance of 20% to change visibility to false
		
		Background.Set(number: number, uniqe: boolean, number_mode: NumberMode, set_mode: SetMode, direction: Direction)
			- 'number' - number (or fragment) of cells that will change visibility
					   - affected by 'number_mode'
			- 'uniqe' - defines if cell with tag 'uniqe' should be changed based on 'set_mode'
			          - if it's false these cells are ignored
			- 'number_mode' - defines if 'number' is number (from 1 to number of all cells) or percentage (from 0 to 1)
			- 'set_mode' - setting for each cell if it will be visible or not
			             - 'show' - visible
			             - 'clear' - not visible
			- 'direction' - random - choose position randomly
						  - horizontal - choose position as a row strating from top left corner
						  - vertical - choose position as a column starting from top left corner
			
			- Change visibility by fixed amount of cells
			
			USE:
				local module = require(%path%.Controller)
				module.Background.Set(0.5, false, module.Enumerators.NumberMode.percentage, module.Enumerators.SetMode.clear, module.Enumerators.Direction.random)
				--Choose half of all cells at random position and set their visibility to false
		
		Background.SetGlobal(uniqe: boolean, set_mode: SetMode)
			- 'uniqe' - defines if cell with tag 'uniqe' should be changed based on 'set_mode'
			          - if it's false these cells are ignored
			- 'set_mode' - setting for each cell if it will be visible or not
			             - 'show' - visible
			             - 'clear' - not visible
			
			- Affect all cells based on 'uniqe' argument
			- This function will set visibility of each cell at once
			
			USE:
				local module = require(%path%.Controller)
				module.Background.SetGlobal(false, module.Enumerators.SetMode.clear) --set visibility of all cell without uniqe tag to false
		
		Background.SetPoint(x: number, y: number, set_mode: SetMode)
			- 'x' - define x position starting from 1
			- 'y' - define y position starting from 1
			- 'set_mode' - setting for each cell if it will be visible or not
			             - 'show' - visible
			             - 'clear' - not visible
			
			- Position (1, 1) is at top left corner
			- Set visibility of cell individualy (uniqe tag is ignored!, but you can check cell if it has uniqe tag)
			- Great tool for creating custom background animations
			USE:
				local module = require(%path%.Controller)
				module.Background.SetPoint(1, 1, module.Enumerators.SetMode.clear) --set visibility of cell at position (1, 1) to false
		
		Background.IsUniqe(x: number, y: number)
			- 'x' - define x position starting from 1
			- 'y' - define y position starting from 1
			
			- Position (1, 1) is at top left corner
			- Return true or false if the cell has uniqe tag or not
			USE:
				local module = require(%path%.Controller)
				print(module.Background.IsUniqe(1, 1)) --check if cell at position (1, 1) has uniqe tag
