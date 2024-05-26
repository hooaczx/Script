if not _G.require then
	_G.require = require
end

--// API References
local GUIData = (function()
	-- Variables
	_V = 1.11
	
	local screenGui = (script:FindFirstChild("ScreenGui")) or game:GetObjects("rbxassetid://2718157603")[1]:FindFirstChild("ScreenGui", true)
	  local Container = screenGui.Frame
	    local Opt = Container.OptionsFrame
	    local Checkbox = Opt.Checkbox
	    local Color = Opt.Color
	    local Frame = Opt.Frame
	    local Execute = Opt.Execute
	    local Mode = Opt.Mode
	    local Number = Opt.Number
	    local Toggle = Opt.Toggle
	  local Mods = screenGui.Mods
	    local ModLabel = Mods.Example
	
	local TextService = game:GetService("TextService")
	local UserInputService = game:GetService("UserInputService")
	local HttpService = game:GetService("HttpService")
	local Player = game:GetService("Players").LocalPlayer
	  local Mouse = Player:GetMouse()
	
	pcall(function()
		screenGui.Parent = game:GetService("CoreGui")
	end)
	
	Container.Parent = nil
	Checkbox.Parent = nil
	Color.Parent = nil
	Frame.Parent = nil
	Execute.Parent = nil
	Mode.Parent = nil
	Number.Parent = nil
	Toggle.Parent = nil
	ModLabel.Parent = nil
	
	local settingsArray = {{Object = screenGui},{}}
	local saveData = {Options = {}, Hotkeys = {}}
	
	local hotkeyFunctions = {}
	local gui = {}
	
	-- Save Functions
	local writefile = writefile or function() end
	local function Save()
		local JSONData = HttpService:JSONEncode(saveData)
		writefile("OpenGui.txt", JSONData)
	end
	
	-- Color Functions
	local color = {}
	local colors = {
		TextDisabled = Color3.fromRGB(200, 200, 200),
		TextEnabled = Color3.fromRGB(255, 255, 255),
	}
	
	local Colors = {
		Color3.fromRGB(255, 73, 73),
		Color3.fromRGB(255, 161, 66),
		Color3.fromRGB(255, 233, 62),
		Color3.fromRGB(137, 255, 64),
		Color3.fromRGB(64, 255, 140),
		Color3.fromRGB(66, 252, 255),
		Color3.fromRGB(64, 147, 255),
		Color3.fromRGB(255, 93, 253),
		Color3.fromRGB(195, 110, 255),
		Color3.fromRGB(255, 90, 134),
		Color3.fromRGB(255, 255, 255),
		Color3.fromRGB(209, 209, 209),
	}
	
	local function h2rgb(m1, m2, h)
		if h<0 then h = h+1 end
		if h>1 then h = h-1 end
		if h*6<1 then
			return m1+(m2-m1)*h*6
		elseif h*2<1 then
			return m2
		elseif h*3<2 then
			return m1+(m2-m1)*(2/3-h)*6
		else
			return m1
		end
	end
	
	function color:rgbToHsv(r, g, b)
		local a = 0
		r, g, b, a = r / 255, g / 255, b / 255, a / 255
		local max, min = math.max(r, g, b), math.min(r, g, b)
		local h, s, v
		v = max
	
		local d = max - min
		if max == 0 then s = 0 else s = d / max end
	
		if max == min then
			h = 0 -- achromatic
		else
			if max == r then
			h = (g - b) / d
			if g < b then h = h + 6 end
			elseif max == g then h = (b - r) / d + 2
			elseif max == b then h = (r - g) / d + 4
			end
			h = h / 6
		end
	
		return h, s, v
	end
	
	function color:hslToRgb(h, s, L)
		h = h / 360
		local m2 = L <= .5 and L*(s+1) or L+s-L*s
		local m1 = L*2-m2
		return
			h2rgb(m1, m2, h+1/3),
			h2rgb(m1, m2, h),
			h2rgb(m1, m2, h-1/3)
	end
	
	function color:rgbToHsl(r, g, b)
		local min = math.min(r, g, b)
		local max = math.max(r, g, b)
		local delta = max - min
	
		local h, s, l = 0, 0, (min + max) / 2
	
		if l > 0 and l < 0.5 then s = delta / (max + min) end
		if l >= 0.5 and l < 1 then s = delta / (2 - max - min) end
	
		if delta > 0 then
			if max == r and max ~= g then h = h + (g-b) / delta end
			if max == g and max ~= b then h = h + 2 + (b-r) / delta end
			if max == b and max ~= r then h = h + 4 + (r-g) / delta end
			h = h / 6
		end
	
		if h < 0 then h = h + 1 end
		if h > 1 then h = h - 1 end
	
		return h * 360, s, l
	end
	
	function color:adjustLightness(color3, amount)
		local h, s, l = self:rgbToHsl(color3.r, color3.g, color3.b)
		return Color3.new(self:hslToRgb(h, s, l + amount))
	end
	
	-- UI Functions
	function gui.tween(object,style,direction,t,goal)
	    local tweenservice = game:GetService("TweenService")
	    local tweenInfo = TweenInfo.new(t,Enum.EasingStyle[style],Enum.EasingDirection[direction])
	    local tween = tweenservice:Create(object,tweenInfo,goal)
		tween.Completed:Connect(function()
			tween:Destroy()
		end)
	    tween:Play()
	    return tween
	end
	
	function gui:makeDraggable(ui, callback)
		local UserInputService = game:GetService("UserInputService")
		
		local dragging
		local dragInput
		local dragStart
		local startPos
		
		local function update(input)
			local delta = input.Position - dragStart
			ui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
			
			if callback then
				callback(ui.Position.X.Offset, ui.Position.Y.Offset)
			end
		end
		
		ui.InputBegan:Connect(function(input)
			if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
				dragging = true
				dragStart = input.Position
				startPos = ui.Position
				
				input.Changed:Connect(function()
					if input.UserInputState == Enum.UserInputState.End then
						dragging = false
					end
				end)
			end
		end)
		
		ui.InputChanged:Connect(function(input)
			if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
				dragInput = input
			end
		end)
		
		UserInputService.InputChanged:Connect(function(input)
			if input == dragInput and dragging then
				update(input)
			end
		end)
	end
	
	function gui:unpack(data, type)
		if data == nil then return end
		if type == "UDim2" then
			return data and UDim2.new(data[1], data[2], data[3], data[4])
		elseif type == "boolean" then
			return data == 1 and true or false
		elseif type == "Color3" then
			return Color3.new(data[1], data[2], data[3])
		end
		return data
	end
	
	function gui:pack(data)
		if typeof(data) == "UDim2" then
			return {data.X.Scale, data.X.Offset, data.Y.Scale, data.Y.Offset}
		elseif typeof(data) == "boolean" then
			return data and 1 or 0
		elseif typeof(data) == "Color3" then
			return {data.r, data.g, data.b}
		end
		return data
	end
	
	function gui:getn(array)
		local n = 0
		for _, v in pairs(array) do
			n = n + 1
		end
		return n
	end
	
	function gui:setText(textLabel, text)
		text = tostring(text)
		textLabel.Text = text
		if textLabel:FindFirstChild("Opaque") then
			textLabel.Opaque.Text = text
		end
		if textLabel:FindFirstChild("Shadow") then
			textLabel.Shadow.Text = text
		end
	end
	
	function gui:onDoubleTap(guiObject, callback)
		local lastTap = tick()
		guiObject.InputBegan:Connect(function(input)
			if input.UserInputType == Enum.UserInputType.MouseButton1 then
				if tick() - lastTap < 0.25 then
					callback()
				end
				lastTap = tick()
			end
		end)
	end
	
	local connections = {}
	function gui:connect(func)
		table.insert(connections, func)
	end
	
	function gui:createList(guiObject, guiData)
		local ListLayout = guiObject.OptionsFrame.UIListLayout
		local Padding = guiObject.OptionsFrame.UIPadding
		guiObject.OptionsFrame.UIListLayout.Changed:Connect(function(Property)
			if Property == "AbsoluteContentSize" then
				guiData.ySize = ListLayout.AbsoluteContentSize.Y + 2 + Padding.PaddingTop.Offset + ListLayout.Padding.Offset * 2
			end
		end)
		
		gui:connect(function()
			if guiObject:FindFirstChild("Title") then
				local yRes = Mouse.ViewSizeY
				local diff = yRes - (guiData.yPos + guiData.ySize)
				local difference = math.clamp(diff, 0, 5000)
				guiObject.OptionsFrame.CanvasSize = UDim2.new(1, 0, 1.001, guiData.ySize - 35)
				
				if guiData.Open then
					guiObject.OptionsFrame.Size = guiObject.OptionsFrame.Size:Lerp(UDim2.new(1, 0, 0, guiData.ySize + math.clamp(diff, -5000, 0)), 0.3)
				else
					guiObject.OptionsFrame.Size = guiObject.OptionsFrame.Size:Lerp(UDim2.new(1, 0, 0, 0), 0.3)
				end
				
				guiObject.Frame.Size = guiObject.OptionsFrame.Size
			else
				if guiData.Open then
					guiObject.Size = guiObject.Size:Lerp(UDim2.new(1, -8, 0, 35 + guiData.ySize), 0.3)
				else
					guiObject.Size = guiObject.Size:Lerp(UDim2.new(1, -8, 0, 35), 0.3)
				end
			end
		end)
		
		if guiObject:FindFirstChild("Dropdown") then
			guiObject.Dropdown.Visible = false
			guiObject.Dropdown.MouseButton1Down:Connect(function()
				guiData.Open = not guiData.Open
				if guiData.Open then
					guiObject.Dropdown.Image = "rbxassetid://3559638428"
				else
					guiObject.Dropdown.Image = "rbxassetid://3554238678"
				end
			end)
		else
			gui:onDoubleTap(guiObject, function()
				guiData.Open = not guiData.Open
			end)
		end
	end
	
	function gui:textColorOnHover(guiObject, guiData)
		local hover = guiData.baseColor or guiObject.TextColor3
		local default = color:adjustLightness(hover, -0.075)
		local mdown = color:adjustLightness(hover, -0.15)
		local mouseIn
		
		local function update()
			if guiData.baseColor then
				hover = guiData.baseColor or guiObject.TextColor3
				default = color:adjustLightness(hover, -0.075)
				mdown = color:adjustLightness(hover, -0.15)
			end
		end
		
		guiObject.MouseEnter:Connect(function()
			update()
			gui.tween(guiObject, "Sine", "Out", 0.25, {
				TextColor3 = hover;
			})
			mouseIn = true
		end)
		
		guiObject.MouseLeave:Connect(function()
			mouseIn = false
			update()
			gui.tween(guiObject, "Sine", "Out", 0.25, {
				TextColor3 = default;
			})
		end)
		
		guiObject.InputBegan:Connect(function(input)
			if input.UserInputType == Enum.UserInputType.MouseButton1 then
				update()
				gui.tween(guiObject, "Sine", "Out", 0.25, {
					TextColor3 = mdown;
				})
			end
		end)
		
		guiObject.InputEnded:Connect(function(input)
			if input.UserInputType == Enum.UserInputType.MouseButton1 then
				update()
				gui.tween(guiObject, "Sine", "Out", 0.25, {
					TextColor3 = mouseIn and hover or default;
				})
			end
		end)
		
		guiObject.TextColor3 = default
	end
	
	function gui:sliderXY(sliderFrame, inputObjects, callback)
		local inputDown = false
		
		local function refresh(c1, c2)
			local sliderX = sliderFrame.AbsolutePosition.X + sliderFrame.AbsoluteSize.X
			local sliderY = sliderFrame.AbsolutePosition.Y + sliderFrame.AbsoluteSize.Y
			
			local distX = sliderX - Mouse.X
			local distY = sliderY - Mouse.Y
			
			local deltaX = 1-math.clamp(distX / sliderFrame.AbsoluteSize.X, 0, 1)
			local deltaY = 1-math.clamp(distY / sliderFrame.AbsoluteSize.Y, 0, 1)
			
			if callback then
				callback(c1 or deltaX, c2 or deltaY)
			end
		end
		
		for _, obj in pairs(inputObjects) do
			obj.InputBegan:Connect(function(input)
				if input.UserInputType == Enum.UserInputType.MouseButton1 then
					inputDown = true
					refresh()
				end
			end)
			obj.InputEnded:Connect(function(input)
				if input.UserInputType == Enum.UserInputType.MouseButton1 then
					inputDown = false
					refresh()
				end
			end)
			obj.InputChanged:Connect(function(input)
				if input == nil or input.UserInputType == Enum.UserInputType.MouseMovement then
					if inputDown then
						refresh()
					end
				end
			end)
		end
		
		return refresh
	end
	
	function gui:createSlider(sliderFrame, inputObjects, callback)
		local slider = sliderFrame.Value
		local inputDown = false
		
		local absPos = sliderFrame.AbsolutePosition.X + sliderFrame.AbsoluteSize.X
		local absSize = sliderFrame.AbsoluteSize.X
		
		local function refresh(custom)
			local mouseX = Mouse.X
			local sliderX = absPos
			local dist = sliderX - mouseX
			local delta = 1 - math.clamp(dist / absSize, 0, 1)
			
			if custom then
				delta = custom
			end
			
			slider.Size = UDim2.new(delta, 0, 1, 0)
			if callback then
				callback(delta, custom)
			end
		end
		
		for _, obj in pairs(inputObjects) do
			obj.InputBegan:Connect(function(input)
				if input.UserInputType == Enum.UserInputType.MouseButton1 then
					inputDown = true
					absPos = sliderFrame.AbsolutePosition.X + sliderFrame.AbsoluteSize.X
					absSize = sliderFrame.AbsoluteSize.X
					refresh()
				end
			end)
			obj.InputEnded:Connect(function(input)
				if input.UserInputType == Enum.UserInputType.MouseButton1 then
					inputDown = false
					refresh()
				end
			end)
			obj.InputChanged:Connect(function(input)
				if input == nil or input.UserInputType == Enum.UserInputType.MouseMovement then
					if inputDown then
						refresh()
					end
				end
			end)
		end
		
		return refresh
	end
	
	function gui:textSize(txt, vSize)
		return TextService:GetTextSize(txt.Text, txt.TextSize, txt.Font, vSize)
	end
	
	local currentHint = 0
	
	function gui:addHint(guiObject, hintText)
		local hintKey = math.random()
		guiObject.MouseEnter:Connect(function()
			hintKey = math.random()
			currentHint = hintKey
			
			wait(2)
			
			if currentHint == hintKey then
				gui:setText(screenGui.Hint, " " .. hintText .. " ")
				local textSize = gui:textSize(screenGui.Hint, Vector2.new(2500, 500))
				screenGui.Hint.Position = UDim2.new(0, Mouse.X, 0, Mouse.Y + 4)
				screenGui.Hint.Size = UDim2.new(0, textSize.X, 0, textSize.Y)
				screenGui.Hint.Visible = true
			end
		end)
		
		guiObject.MouseLeave:Connect(function()
			hintKey = math.random()
		end)
		
		Mouse.Move:Connect(function()
			if currentHint == hintKey then
				screenGui.Hint.Visible = false
			end
		end)
	end
	
	-- UI Type Library
	local lib = {}
	
	function lib.Color(data, dataArray)
		local guiObject = Color:Clone()
		local color3Value = gui:unpack(saveData.Options[data.ID].Value, "Color3") or data.Default or Color3.new(1, 1, 1)
		local guiData = {}
		
		local HSV = color3Value
		local H, S, V = color:rgbToHsv(HSV.r * 255, HSV.g * 255, HSV.b * 255)
		
		local newValue = function()
			HSV = Color3.fromHSV(H, S, V)
			guiObject.SV.BackgroundColor3 = Color3.fromHSV(H, 1, 1)
			guiObject.Indicator.BackgroundColor3 = HSV
			saveData.Options[data.ID].Value = gui:pack(HSV, "Color3")
			
			guiObject.R.Text = math.floor(HSV.r * 255)
			guiObject.G.Text = math.floor(HSV.g * 255)
			guiObject.B.Text = math.floor(HSV.b * 255)
			
			if data.Callback then
				data.Callback(HSV)
			end
		end
		
		local function updateHSV()
			H, S, V = color:rgbToHsv(HSV.r * 255, HSV.g * 255, HSV.b * 255)
		end
		
		local H_set = gui:sliderXY(guiObject.H, {guiObject.H}, function(X, Y, u)
			if not u then H = 1 - Y end
			guiObject.H.Locator.Position = UDim2.new(0.5, 0, Y, 0)
			newValue()
		end)
		
		local SV_set = gui:sliderXY(guiObject.SV, {guiObject.SV}, function(X, Y, u)
			if not u then S = X; V = 1 - Y; end
			guiObject.SV.Locator.Position = UDim2.new(X, 0, Y, 0)
			newValue()
		end)
		
		guiObject.R.FocusLost:Connect(function()
			HSV = Color3.new(guiObject.R.Text / 255, HSV.g, HSV.b)
			updateHSV()
			newValue()
		end)
		guiObject.G.FocusLost:Connect(function()
			HSV = Color3.new(HSV.r, guiObject.G.Text / 255, HSV.b)
			updateHSV()
			newValue()
		end)
		guiObject.B.FocusLost:Connect(function()
			HSV = Color3.new(HSV.r, HSV.g, guiObject.B.Text / 255)
			updateHSV()
			newValue()
		end)
		
		newValue()
		SV_set(S, 1 - V)
		H_set(0, H)
		
		guiData.ySize = 0
		guiData.Open = false
		guiData.baseColor = colors.TextEnabled
		
		gui:setText(guiObject.Label, data.Name)
		gui:textColorOnHover(guiObject.Label, guiData)
		
		return guiObject
	end
	
	function lib.Number(data, dataArray)
		local guiObject = Number:Clone()
		local Value = gui:unpack(saveData.Options[data.ID].Value, "number") or data.Default or math.floor(data.Min + (data.Max - data.Min) / 2)
		local guiData = {}
		
		local dMax = data.Max - data.Min
		local dValue = (Value - data.Min) / dMax
		
		data.Round = data.Round or 1
		
		local newValue = function(delta)
			local exactValue = data.Min + (data.Max - data.Min) * delta
			Value = math.floor(exactValue / data.Round) * data.Round
			Value = math.clamp(Value, data.Min, data.Max)
			guiObject.Indicator.Value.Text = tostring(Value)
			
			if data.Callback then
				data.Callback(Value)
			end
			saveData.Options[data.ID].Value = gui:pack(Value, "number")
		end
		
		local slideSet = gui:createSlider(guiObject.ValueFrame, {guiObject.Label, guiObject.Indicator}, newValue)
		slideSet(math.clamp(dValue, 0, 1))
		
		guiObject.Indicator.MouseButton1Down:Connect(newValue)
		guiObject.Label.MouseButton1Down:Connect(newValue)
		newValue(math.clamp(dValue, 0, 1))
		
		guiData.ySize = 0
		guiData.Open = false
		guiData.baseColor = colors.TextEnabled
		
		gui:createList(guiObject, guiData)
		gui:setText(guiObject.Label, data.Name)
		gui:textColorOnHover(guiObject.Label, guiData)
		
		return guiObject
	end
	
	function lib.Execute(data, dataArray)
		local guiObject = Execute:Clone()
		local guiData = {}
		
		local newValue = function(val)
			if data.Callback then
				data.Callback()
			end
		end
		
		guiObject.MouseEnter:Connect(function()
			gui.tween(guiObject.Indicator, "Sine", "Out", .25, {Size = UDim2.new(0, 40, 0, 25)})
		end)
		
		guiObject.MouseLeave:Connect(function()
			gui.tween(guiObject.Indicator, "Sine", "Out", .25, {Size = UDim2.new(0, 0, 0, 25)})
		end)
		
		guiObject.Indicator.MouseButton1Down:Connect(newValue)
		guiObject.Label.MouseButton1Down:Connect(newValue)
		newValue(true)
		
		guiData.ySize = 0
		guiData.Open = false
		guiData.baseColor = colors.TextEnabled
		
		gui:createList(guiObject, guiData)
		gui:setText(guiObject.Label, data.Name)
		gui:textColorOnHover(guiObject.Label, guiData)
		
		return guiObject
	end
	
	function lib.Mode(data, dataArray)
		local guiObject = Mode:Clone()
		local valueIndex = gui:unpack(saveData.Options[data.ID].Value, "number") or data.Default or 1
		local guiData = {}
		
		local newValue = function(val)
			if val == true then else
				valueIndex = (valueIndex % #data.Modes) + 1
			end
			
			local Value = data.Modes[valueIndex]
			gui:setText(guiObject.Mode, Value)
			
			if data.Callback then
				data.Callback(Value)
			end
			saveData.Options[data.ID].Value = gui:pack(valueIndex)
		end
		
		guiObject.Mode.MouseButton1Down:Connect(newValue)
		guiObject.Label.MouseButton1Down:Connect(newValue)
		newValue(true)
		
		guiData.ySize = 0
		guiData.Open = false
		guiData.baseColor = colors.TextEnabled
		
		gui:createList(guiObject, guiData)
		gui:setText(guiObject.Label, data.Name)
		gui:textColorOnHover(guiObject.Label, guiData)
		
		return guiObject
	end
	
	function lib.Hotkey(data, dataArray)
		local guiObject = Mode:Clone()
		local hotkeyInput = gui:unpack(saveData.Hotkeys[data.ID], "string") or data.Hotkey or ""
		local guiData = {}
		
		local lastInput = hotkeyInput
		local mouseIn = false
		
		guiObject.Name = "Z"
		
		local newValue = function(val)
			local input
			gui:setText(guiObject.Mode, "...")
			saveData.Hotkeys[data.ID] = nil
			
			if not val then
				input = lastInput
				mouseIn = true
				
				lastInput = nil
				
				repeat wait() until mouseIn == false or lastInput
			end
			
			if not lastInput then
				lastInput = hotkeyInput
			end
			
			saveData.Hotkeys[data.ID] = tostring(lastInput)
			hotkeyFunctions[data.ID] = data.callback
			
			hotkeyInput = tostring(lastInput)
			saveData.Options[data.ID].Value = hotkeyInput
			gui:setText(guiObject.Mode, hotkeyInput:sub(14))
		end
		
		UserInputService.InputBegan:Connect(function(input)
			if input.KeyCode == Enum.KeyCode.Unknown then return end
			if input.KeyCode == Enum.KeyCode.Backspace then lastInput = "" return end
			lastInput = tostring(input.KeyCode)
		end)
		
		guiObject.Mode.MouseButton1Down:Connect(function() newValue() end)
		guiObject.Label.MouseButton1Down:Connect(function() newValue() end)
		guiObject.MouseLeave:Connect(function()
			mouseIn = false
		end)
		newValue(true)
		
		guiData.ySize = 0
		guiData.Open = false
		guiData.baseColor = colors.TextEnabled
		
		gui:createList(guiObject, guiData)
		gui:setText(guiObject.Label, "Hotkey")
		gui:textColorOnHover(guiObject.Label, guiData)
		
		guiObject.Parent = dataArray.Object.OptionsFrame
		
		return guiObject
	end
	
	function lib.Toggle(data, dataArray)
		local guiObject = Toggle:Clone()
		local Value = gui:unpack(saveData.Options[data.ID].Value, "boolean") or data.Default or false
		local guiData = {}
		
		local modFrame = ModLabel:Clone()
		modFrame.Parent = Mods
		modFrame.TextColor3 = Colors[math.random(1, #Colors)]
		modFrame.Visible = false
		gui:setText(modFrame, data.Name)
		
		guiObject.Name = data.Name
		
		local newValue = function(val, set)
			if val == true then
			else
				if set then
					Value = set
				else
					Value = not Value
				end
			end
			
			if Value then
				gui.tween(guiObject.Indicator, "Sine", "Out", .25, {BackgroundColor3 = Color3.fromRGB(60, 222, 60)})
				guiObject.Indicator.Text = "ON"
				guiData.baseColor = colors.TextEnabled
			else
				gui.tween(guiObject.Indicator, "Sine", "Out", .25, {BackgroundColor3 = Color3.fromRGB(222, 60, 60)})
				guiObject.Indicator.Text = "OFF"
				guiData.baseColor = colors.TextDisabled
			end
			
			if data.Callback then
				data.Callback(Value)
			end
			
			saveData.Options[data.ID].Value = gui:pack(Value)
			modFrame.Visible = Value
			
		end
		
		guiObject.MouseEnter:Connect(function()
			gui.tween(guiObject.Indicator, "Sine", "Out", .25, {Size = UDim2.new(0, 40, 0, 25)})
		end)
		
		guiObject.MouseLeave:Connect(function()
			gui.tween(guiObject.Indicator, "Sine", "Out", .25, {Size = UDim2.new(0, 0, 0, 25)})
		end)
		
		gui.tween(guiObject.Indicator, "Sine", "Out", .25, {Size = UDim2.new(0, 0, 0, 25)})
		guiObject.Indicator.MouseButton1Down:Connect(function() newValue() end)
		guiObject.Label.MouseButton1Down:Connect(function() newValue() end)
		newValue(true)
		
		guiData.ySize = 0
		guiData.Open = false
		guiData.baseColor = colors.TextDisabled
		
		gui:createList(guiObject, guiData)
		gui:setText(guiObject.Label, data.Name)
		gui:textColorOnHover(guiObject.Label, guiData)
		
		data.callback = newValue
		
		return guiObject
	end
	
	function lib.Checkbox(data, dataArray)
		local guiObject = Checkbox:Clone()
		local Value = gui:unpack(saveData.Options[data.ID].Value, "boolean") or data.Default or false
		local guiData = {}
		
		guiObject.Name = "0"
		
		local newValue = function(val)
			if val == true then else
				Value = not Value
			end
			if Value then
				gui.tween(guiObject.Indicator, "Sine", "Out", .35, {Size = UDim2.new(0, 35, 0, 35)})
				guiData.baseColor = colors.TextEnabled
			else
				gui.tween(guiObject.Indicator, "Sine", "Out", .35, {Size = UDim2.new(0, 0, 0, 35)})
				guiData.baseColor = colors.TextDisabled
			end
			if data.Callback then
				data.Callback(Value)
			end
			saveData.Options[data.ID].Value = gui:pack(Value)
		end
		
		guiObject.Indicator.MouseButton1Down:Connect(newValue)
		guiObject.Label.MouseButton1Down:Connect(newValue)
		newValue(true)
		
		guiData.ySize = 0
		guiData.Open = false
		guiData.baseColor = colors.TextDisabled
		
		gui:createList(guiObject, guiData)
		gui:setText(guiObject.Label, data.Name)
		gui:textColorOnHover(guiObject.Label, guiData)
		
		return guiObject
	end
	
	function lib.Frame(data, dataArray)
		local guiObject = Frame:Clone()
		
		local guiData = {}
		guiData.ySize = 0
		guiData.Open = false
		
		gui:createList(guiObject, guiData)
		gui:setText(guiObject.Label, data.Name)
		gui:textColorOnHover(guiObject.Label, guiData)
		
		return guiObject
	end
	
	function lib.Container(data, dataArray)
		local guiObject = Container:Clone()
		
		guiObject.Position = gui:unpack(saveData.Options[data.ID].Position, "UDim2") or UDim2.new(0, 3, 0, 3 + gui:getn(settingsArray[2]) * 38)
		
		local guiData = {}
		guiData.yPos = 0
		guiData.ySize = 0
		guiData.Open = false
		
		gui:textColorOnHover(guiObject.Title, guiData)
		gui:createList(guiObject, guiData)
		gui:setText(guiObject.Title, data.Name)
		gui:makeDraggable(guiObject, function(x, y)
			guiData.yPos = y
			saveData.Options[data.ID].Position = gui:pack(guiObject.Position)
		end)
		
		return guiObject
	end
	
	-- UI Creation Library
	function gui.create(self, guiType, data)
		if self == gui then
			self = settingsArray
		end
		
		data.ID = data.Name .. "_" .. (self[1].Name or "TOP")
		
		if not saveData.Options[data.ID] then
			saveData.Options[data.ID] = {}
		end
		
		if self[1].Object:FindFirstChild("Dropdown") then
			self[1].Object.Dropdown.Visible = true
		end
		
		local dataArray = {}
		local objectArray = {}
		local selfArray = {dataArray, objectArray, create = gui.create, callback = data.Callback}
		dataArray.Name = data.Name
		dataArray.Data = data
		dataArray.Object = lib[guiType](data, dataArray)
		dataArray.self = selfArray
		
		if guiType == "Toggle" then
			lib.Hotkey(data, dataArray)
		end
		if data.Hint then
			local Object = dataArray.Object
			gui:addHint(Object:FindFirstChild("Title") or Object:FindFirstChild("Label"), data.Hint)
		end
		
		self[1][data.Name] = selfArray
		self[2][data.Name] = dataArray.Object
		
		dataArray.Object.Parent = self[1].Object:FindFirstChild("OptionsFrame") or self[1].Object
		
		return dataArray
	end
	
	-- Connection Stuff
	game:GetService("RunService").RenderStepped:Connect(function()
		for _, func in pairs(connections) do
			func()
		end
	end)
	
	UserInputService.InputBegan:Connect(function(input, gameProcessed)
		if gameProcessed then return end
		for id, key in pairs(saveData.Hotkeys) do
			if key == tostring(input.KeyCode) then
				hotkeyFunctions[id]()
			end
		end
	end)
	
	Mods.Text = "OpenGui " .. _V
	
	game.Close:Connect(function()
		Save()
	end)
	
	return {gui, saveData, screenGui}
end)()

local _ESP = (function()
	--// Variables
	local RunService = game:GetService("RunService")
	local Players = game:GetService("Players")
	  local Player = Players.LocalPlayer
	local Screen = Instance.new("ScreenGui")
	  local Viewport = Instance.new("ViewportFrame", Screen)
	
	local module = {}
	local characters = {}
	local clones = {}
	local parts = {}
	
	module.Options = {
		Enabled = false,
		Parent = script.Parent or game.CoreGui,
		Color = Color3.new(1, 1, 1),
		ShowDescendants = false,
		TeamColor = false,
		ShowSelf = false,
		ShowTeam = false,
		Mode = "Shader",
		Opacity = 1,
		Arrow = false,
		MaxDistance = 500,
	}
	
	--// Edits
	Viewport.Size = UDim2.new(1, 0, 1, 0)
	Viewport.BackgroundTransparency = 1
	Viewport.CurrentCamera = workspace.CurrentCamera
	Screen.IgnoreGuiInset = true
	
	--// Functions
	local function getParts(Model)
		local parts = {}
		local descendants = Model:GetDescendants()
		local descendantsn = #descendants
		for i = 1, descendantsn do
			local desc = descendants[i]
			if desc:IsA("BasePart") then
				table.insert(parts, desc)
			end
		end
		return parts
	end
	
	local function getPart(Model)
		return Model.PrimaryPart or Model:FindFirstChild("HumanoidRootPart") or Model:FindFirstChildWhichIsA("Part")
	end
	
	function module:Clone(Object)
		local isArchivable = Object.Archivable
		local Clone
		
		Object.Archivable = true
		Clone = Object:Clone()
		Object.Archivable = isArchivable
		
		for _, child in pairs(Clone:GetDescendants()) do
			if child:IsA("Clothing") or child:IsA("Decal") or child:IsA("Script") or child:IsA("LocalScript") or child:IsA("Sound") then
				child:Destroy()
			elseif child:IsA("BasePart") then
				child.Color = Color3.new(1, 1, 1)
				child.Material = "ForceField"
			elseif child:IsA("Humanoid") then
				child.DisplayDistanceType = "None"
			elseif child:IsA("SpecialMesh") then
				child.TextureId = "rbxassetid://55054494"
			elseif child:IsA("MeshPart") then
				child.TextureID = "rbxassetid://55054494"
			end
		end
		
		return Clone
	end
	
	function module:Enable()
		module.Options.Enabled = true
		Screen.Parent = module.Options.Parent
		
		module:ReloadCharacters()
	end
	
	function module:Disable()
		module.Options.Enabled = false
		Screen.Parent = nil
	end
	
	function module:ReloadCharacters()
		Viewport:ClearAllChildren()
		if module.Options.Mode ~= "Shader" then
			return
		end
		for player, character in pairs(characters) do
			local clone = module:Clone(character)
			clone.Name = player.Name
			clone.Parent = Viewport
			clones[player] = clone
		end
	end
	
	local function newPlayer(player)
		if player.Character then
			characters[player] = player.Character
			
			local clone = module:Clone(player.Character)
			clone.Name = player.Name
			clone.Parent = Viewport
			clones[player] = clone
		end
		player.CharacterAdded:Connect(function(char)
			if clones[player] then
				clones[player]:Destroy()
				clones[player] = nil
			end;if characters[player] then
				characters[player]:Destroy()
				characters[player] = nil
			end
			
			characters[player] = char
			
			local clone = module:Clone(char)
			clone.Name = player.Name
			clone.Parent = Viewport
			clones[player] = clone
		end)
	end
	
	Players.PlayerAdded:Connect(newPlayer)
	Players.PlayerRemoving:Connect(function(player)
		if clones[player] then
			clones[player]:Destroy()
			clones[player] = nil
		end;if characters[player] then
			characters[player]:Destroy()
			characters[player] = nil
		end
	end)
	for _, player in pairs(Players:GetPlayers()) do
		newPlayer(player)
	end
	
	RunService.RenderStepped:Connect(function()
		if module.Options.Enabled and module.Options.Mode == "Shader" then
			for player, character in pairs(characters) do
				local clone = clones[player]
				local target = getPart(clone)
				if target then
					if ((player.Team == Player.Team and module.Options.ShowTeam) or player.Team ~= Player.Team) and (target.Position - workspace.CurrentCamera.CFrame.p).Magnitude <= module.Options.MaxDistance then
						if (player == Player and module.Options.ShowSelf) or player ~= Player then
							local parts = getParts(clone)
							for i = 1, #parts do
								local obj = parts[i]
								local cor = character:FindFirstChild(obj.Name, true)
								if character:FindFirstChild(obj.Parent.Name) then
									cor = character:FindFirstChild(obj.Parent.Name):FindFirstChild(obj.Name)
								end
								
								if cor and obj then
									if module.Options.TeamColor then
										obj.Color = player.TeamColor.Color
									else
										obj.Color = Color3.new(1, 1, 1)
									end
									if module.Options.ShowDescendants then
										obj.CFrame = cor.CFrame
									elseif obj.Parent == clone then
										obj.CFrame = cor.CFrame
									else
										obj.CFrame = CFrame.new(10000, 10000, 10000)
									end
								end
							end
							if clone.Parent == nil then
								clone.Parent = Viewport
							end
						else
							clone.Parent = nil
						end
					else
						clone.Parent = nil
					end
				else
					clone.Parent = nil
				end
			end
			Viewport.ImageColor3 = module.Options.Color
			Viewport.ImageTransparency = 1 - module.Options.Opacity
		end
	end)
	
	return module
	
end)()
local _ESP2D = (function()
	--// Variables
	local RunService = game:GetService("RunService")
	local Players = game:GetService("Players")
	  local Player = Players.LocalPlayer
	
	local module = {}
	local characters = {}
	local esp = {}
	
	module.Options = {
		Enabled = false,
		Parent = script.Parent or game.CoreGui,
		Color = Color3.new(1, 1, 1),
		TeamColor = false,
		ShowSelf = false,
		ShowTeam = false,
		ShowDescendants = false,
		Opacity = 1,
		Mode = "Box",
		Arrow = false,
		MaxDistance = 500,
	}
	
	--// Functions
	local function getParts(Model)
		local parts = {}
		local descendants = (module.Options.ShowDescendants and Model:GetDescendants()) or Model:GetChildren()
		local descendantsn = #descendants
		for i = 1, descendantsn do
			local desc = descendants[i]
			if desc:IsA("BasePart") then
				table.insert(parts, desc)
			end
		end
		return parts
	end
	
	local function getPart(Model)
		return Model.PrimaryPart or Model:FindFirstChild("HumanoidRootPart") or Model:FindFirstChildWhichIsA("Part")
	end
	
	function module:Enable()
		module.Options.Enabled = true
		module:ReloadCharacters()
	end
	
	function module:Disable()
		module.Options.Enabled = false
	end
	
	function module:LoadCharacter(player, char)
		local boxes = {}
		if module.Options.Mode == "Default" then
			local parts = getParts(char)
			for i = 1, #parts do
				local part = parts[i]
				local adornment = Instance.new("BoxHandleAdornment", module.Options.Parent)
				adornment.Adornee = part
				adornment.AlwaysOnTop = true
				adornment.Color3 = module.Options.Color
				adornment.Size = part.Size
				adornment.ZIndex = 1
				adornment.Transparency = 1 - module.Options.Opacity
				if module.Options.TeamColor then
					adornment.Color3 = player.TeamColor.Color
				end
				
				table.insert(boxes, adornment)
			end
			
			local part = getPart(char)
			if module.Options.Arrow then
				local arrow = Instance.new("Handles", module.Options.Parent)
				arrow.Adornee = part
				arrow.Faces = Faces.new(Enum.NormalId.Front)
				arrow.Style = Enum.HandlesStyle.Movement
				arrow.Color3 = module.Options.Color
				if module.Options.TeamColor then
					arrow.Color3 = player.TeamColor.Color
				end
				table.insert(boxes, arrow)
			end
		elseif module.Options.Mode == "Box" then
			local part = getPart(char)
			local adornment = Instance.new("BoxHandleAdornment", module.Options.Parent)
			adornment.Adornee = part
			adornment.AlwaysOnTop = true
			adornment.Color3 = module.Options.Color
			adornment.Size = char:GetExtentsSize()
			adornment.ZIndex = 1
			adornment.Transparency = 1 - module.Options.Opacity
			if module.Options.TeamColor then
				adornment.Color3 = player.TeamColor.Color
			end
			
			if module.Options.Arrow then
				local arrow = Instance.new("Handles", module.Options.Parent)
				arrow.Adornee = part
				arrow.Faces = Faces.new(Enum.NormalId.Front)
				arrow.Style = Enum.HandlesStyle.Movement
				arrow.Color3 = module.Options.Color
				if module.Options.TeamColor then
					arrow.Color3 = player.TeamColor.Color
				end
				table.insert(boxes, arrow)
			end
			
			table.insert(boxes, adornment)
		elseif module.Options.Mode == "Square" then
			local part = getPart(char)
			local billboard = (function()
		local partsWithId = {}
		local awaitRef = {}
		
		local root = {
			ID = 0;
			Type = "BillboardGui";
			Properties = {
				ClipsDescendants = true;
				LightInfluence = 1;
				Name = "B";
				ZIndexBehavior = Enum.ZIndexBehavior.Sibling;
				StudsOffset = Vector3.new(0,-0.5,0);
				Active = true;
				AlwaysOnTop = true;
				Size = UDim2.new(5,0,6,0);
			};
			Children = {
				{
					ID = 1;
					Type = "Frame";
					Properties = {
						AnchorPoint = Vector2.new(0.5,0.5);
						BackgroundTransparency = 0.5;
						Position = UDim2.new(0.5,0,0.5,0);
						BorderColor3 = Color3.new(4/51,4/51,4/51);
						Size = UDim2.new(1,-4,1,-4);
						BorderSizePixel = 2;
						BackgroundColor3 = Color3.new(1,1,1);
					};
					Children = {};
				};
			};
		};
		
		local function Scan(item, parent)
			local obj = Instance.new(item.Type)
			if (item.ID) then
				local awaiting = awaitRef[item.ID]
				if (awaiting) then
					awaiting[1][awaiting[2]] = obj
					awaitRef[item.ID] = nil
				else
					partsWithId[item.ID] = obj
				end
			end
			for p,v in pairs(item.Properties) do
				if (type(v) == "string") then
					local id = tonumber(v:match("^_R:(%w+)_$"))
					if (id) then
						if (partsWithId[id]) then
							v = partsWithId[id]
						else
							awaitRef[id] = {obj, p}
							v = nil
						end
					end
				end
				obj[p] = v
			end
			for _,c in pairs(item.Children) do
				Scan(c, obj)
			end
			obj.Parent = parent
			return obj
		end
		
		return function() return Scan(root, nil) end
	end)()()
			billboard.Parent = module.Options.Parent
			billboard.Adornee = part
			billboard.Frame.BackgroundColor3 = module.Options.Color
			billboard.Frame.Transparency = 1 - module.Options.Opacity
			if module.Options.TeamColor then
				billboard.Frame.Color3 = player.TeamColor.Color
			end
			
			if module.Options.Arrow then
				local arrow = Instance.new("Handles", module.Options.Parent)
				arrow.Adornee = part
				arrow.Faces = Faces.new(Enum.NormalId.Front)
				arrow.Style = Enum.HandlesStyle.Movement
				arrow.Color3 = module.Options.Color
				if module.Options.TeamColor then
					arrow.Color3 = player.TeamColor.Color
				end
				table.insert(boxes, arrow)
			end
			
			table.insert(boxes, billboard)
		end
		esp[player] = boxes
	end
	
	function module:ReloadCharacters()
		for plr, tbl in pairs(esp) do
			for i, v in pairs(tbl) do
				v:Destroy()
			end
			esp[plr] = {}
		end
		if module.Options.Enabled then
			for player, character in pairs(characters) do
				local target = getPart(character)
				if target then
					if ((player.Team == Player.Team and module.Options.ShowTeam) or player.Team ~= Player.Team) and target and (target.Position - workspace.CurrentCamera.CFrame.p).Magnitude <= module.Options.MaxDistance then
						if (player == Player and module.Options.ShowSelf) or player ~= Player then
							module:LoadCharacter(player, character)
						end
					end
				end
			end
		end
	end
	
	local function newPlayer(player)
		if player.Character then
			characters[player] = player.Character
			module:LoadCharacter(player, player.Character)
		end
		player.CharacterAdded:Connect(function(char)
			if esp[player] then
				for i, v in pairs(esp[player]) do
					v:Destroy()
				end
				esp[player] = {}
			end
			
			characters[player] = char
			module:LoadCharacter(player, player.Character)
		end)
	end
	
	Players.PlayerAdded:Connect(newPlayer)
	Players.PlayerRemoving:Connect(function(player)
		if esp[player] then
			for i, v in pairs(esp[player]) do
				v:Destroy()
			end
			esp[player] = {}
			characters[player] = nil
		end
	end)
	for _, player in pairs(Players:GetPlayers()) do
		newPlayer(player)
	end
	
	spawn(function()
		while wait(2) do
			module:ReloadCharacters()
		end
	end)
	
	return module
	
end)()
local _Chams = (function()
	--// Variables
	local RunService = game:GetService("RunService")
	local Players = game:GetService("Players")
	  local Player = Players.LocalPlayer
	local Screen = Instance.new("ScreenGui")
	  local Viewport = Instance.new("ViewportFrame", Screen)
	
	local module = {}
	local characters = {}
	local clones = {}
	local parts = {}
	
	module.Options = {
		Enabled = false,
		Parent = script.Parent or game.CoreGui,
		Color = Color3.new(1, 1, 1),
		ShowDescendants = false,
		TeamColor = false,
		ShowSelf = false,
		ShowTeam = false,
		Mode = "Shader",
		Opacity = 1,
		MaxDistance = 500,
	}
	
	--// Edits
	Viewport.Size = UDim2.new(1, 0, 1, 0)
	Viewport.BackgroundTransparency = 1
	Viewport.CurrentCamera = workspace.CurrentCamera
	Screen.IgnoreGuiInset = true
	
	--// Functions
	local function getParts(Model)
		local parts = {}
		local descendants = Model:GetDescendants()
		local descendantsn = #descendants
		for i = 1, descendantsn do
			local desc = descendants[i]
			if desc:IsA("BasePart") then
				table.insert(parts, desc)
			end
		end
		return parts
	end
	
	local function getPart(Model)
		return Model.PrimaryPart or Model:FindFirstChild("HumanoidRootPart") or Model:FindFirstChildWhichIsA("Part")
	end
	
	function module:Clone(Object)
		local isArchivable = Object.Archivable
		local Clone
		
		Object.Archivable = true
		Clone = Object:Clone()
		Object.Archivable = isArchivable
		
		if module.Options.Mode == "Shader" then
			Viewport.Ambient = Color3.fromRGB(200, 200, 200)
		else
			Viewport.Ambient = Color3.fromRGB(255, 255, 255)
		end
		
		for _, child in pairs(Clone:GetDescendants()) do
			if child:IsA("Script") or child:IsA("LocalScript") or child:IsA("Sound") then
				child:Destroy()
			elseif child:IsA("Humanoid") then
				child.DisplayDistanceType = "None"
			elseif module.Options.Mode ~= "Shader" then
				if child:IsA("SpecialMesh") then
					child.TextureId = ""
				elseif child:IsA("MeshPart") then
					child.TextureID = ""
				elseif child:IsA("BasePart") then
					child.Color = Color3.new(1, 1, 1)
					child.Material = "Neon"
				elseif child:IsA("Clothing") or child:IsA("Decal") then
					child:Destroy()
				end
			end
		end
		
		return Clone
	end
	
	function module:Enable()
		module.Options.Enabled = true
		Screen.Parent = module.Options.Parent
		
		module:ReloadCharacters()
	end
	
	function module:Disable()
		module.Options.Enabled = false
		Screen.Parent = nil
	end
	
	function module:ReloadCharacters()
		Viewport:ClearAllChildren()
		for player, character in pairs(characters) do
			local clone = module:Clone(character)
			clone.Name = player.Name
			clone.Parent = Viewport
			clones[player] = clone
		end
	end
	
	local function newPlayer(player)
		if player.Character then
			characters[player] = player.Character
			
			local clone = module:Clone(player.Character)
			clone.Name = player.Name
			clone.Parent = Viewport
			clones[player] = clone
		end
		player.CharacterAdded:Connect(function(char)
			if clones[player] then
				clones[player]:Destroy()
				clones[player] = nil
			end;if characters[player] then
				characters[player]:Destroy()
				characters[player] = nil
			end
			
			characters[player] = char
			
			local clone = module:Clone(char)
			clone.Name = player.Name
			clone.Parent = Viewport
			clones[player] = clone
		end)
	end
	
	Players.PlayerAdded:Connect(newPlayer)
	Players.PlayerRemoving:Connect(function(player)
		if clones[player] then
			clones[player]:Destroy()
			clones[player] = nil
		end;if characters[player] then
			characters[player]:Destroy()
			characters[player] = nil
		end
	end)
	for _, player in pairs(Players:GetPlayers()) do
		newPlayer(player)
	end
	
	RunService.RenderStepped:Connect(function()
		if module.Options.Enabled then
			for player, character in pairs(characters) do
				local clone = clones[player]
				local target = getPart(clone)
				
				if target then
					if ((player.Team == Player.Team and module.Options.ShowTeam) or player.Team ~= Player.Team) and target and (target.Position - workspace.CurrentCamera.CFrame.p).Magnitude <= module.Options.MaxDistance then
						if (player == Player and module.Options.ShowSelf) or player ~= Player then
							local parts = getParts(clone)
							for i = 1, #parts do
								local obj = parts[i]
								local cor = character:FindFirstChild(obj.Name, true)
								if character:FindFirstChild(obj.Parent.Name) then
									cor = character:FindFirstChild(obj.Parent.Name):FindFirstChild(obj.Name)
								end
								
								if cor and obj then
									if module.Options.TeamColor then
										obj.Color = player.TeamColor.Color
									elseif module.Options.Mode ~= "Shader" then
										obj.Color = Color3.new(1, 1, 1)
									end
									if module.Options.ShowDescendants then
										obj.CFrame = cor.CFrame
									elseif obj.Parent == clone then
										obj.CFrame = cor.CFrame
									else
										obj.CFrame = CFrame.new(10000, 10000, 10000)
									end
								end
							end
							if clone.Parent == nil then
								clone.Parent = Viewport
							end
						else
							clone.Parent = nil
						end
					else
						clone.Parent = nil
					end
				else
					clone.Parent = nil
				end
			end
			Viewport.ImageColor3 = module.Options.Color
			Viewport.ImageTransparency = 1 - module.Options.Opacity
		end
	end)
	
	return module
	
end)()
local _Tracers = (function()
	--// Variables
	local RunService = game:GetService("RunService")
	local Players = game:GetService("Players")
	  local Player = Players.LocalPlayer
	local Screen = Instance.new("ScreenGui")
	local Camera = workspace.CurrentCamera
	
	local module = {}
	local characters = {}
	local tracers = {}
	
	module.Options = {
		Enabled = false,
		Parent = script.Parent or game.CoreGui,
		Color = Color3.new(1, 1, 1),
		TeamColor = false,
		ShowSelf = false,
		ShowTeam = false,
		Opacity = 1,
		Radius = 1,
		MaxDistance = 500,
	}
	
	Screen.Parent = module.Options.Parent
	Screen.IgnoreGuiInset = true
	
	--// Functions
	local function getParts(Model)
		local parts = {}
		local descendants = Model:GetDescendants()
		local descendantsn = #descendants
		for i = 1, descendantsn do
			local desc = descendants[i]
			if desc:IsA("BasePart") then
				table.insert(parts, desc)
			end
		end
		return parts
	end
	
	local function getPart(Model)
		return Model.PrimaryPart or Model:FindFirstChild("HumanoidRootPart") or Model:FindFirstChildWhichIsA("Part")
	end
	
	function module:Enable()
		module.Options.Enabled = true
		module:ReloadCharacters()
	end
	
	function module:Disable()
		module.Options.Enabled = false
		for plr, line in pairs(tracers) do
			if line then
				line[1]:Destroy()
			end
			tracers[plr] = nil
		end
	end
	
	function module:LoadCharacter(player, char)
		local tracer = {}
		local target = getPart(char)
		if target then
			local line = Instance.new("Part", Screen)
			line.Transparency = 1
			line.Anchored = true
			line.CanCollide = false
			
			local adornment = Instance.new("LineHandleAdornment", line)
			adornment.Name = "A"
			adornment.AlwaysOnTop = true
			adornment.ZIndex = 1
			adornment.Adornee = line
			
			tracer[1] = line
			tracer[2] = target
			tracer[3] = player
		else
			return
		end
		
		tracers[player] = tracer
	end
	
	function module:ReloadCharacters()
		for plr, line in pairs(tracers) do
			if line then
				line[1]:Destroy()
			end
			tracers[plr] = nil
		end
		if module.Options.Enabled then
			for player, character in pairs(characters) do
				if (player.Team == Player.Team and module.Options.ShowTeam) or player.Team ~= Player.Team then
					if (player == Player and module.Options.ShowSelf) or player ~= Player then
						module:LoadCharacter(player, character)
					end
				end
			end
		end
	end
	
	local function newPlayer(player)
		if player.Character then
			characters[player] = player.Character
			module:LoadCharacter(player, player.Character)
		end
		player.CharacterAdded:Connect(function(char)
			if tracers[player] then
				tracers[player][1]:Destroy()
				tracers[player] = nil
			end
			char:WaitForChild("Humanoid")
			characters[player] = char
			module:LoadCharacter(player, player.Character)
		end)
	end
	
	Players.PlayerAdded:Connect(newPlayer)
	Players.PlayerRemoving:Connect(function(player)
		if tracers[player] then
			if tracers[player] then
				tracers[player][1]:Destroy()
				tracers[player] = nil
			end
			characters[player] = nil
		end
	end)
	for _, player in pairs(Players:GetPlayers()) do
		newPlayer(player)
	end
	
	local function divideUDim(udim, factor)
		return UDim2.new(udim.X.Scale / factor, udim.X.Offset / factor, udim.Y.Scale / factor, udim.Y.Offset / factor)
	end
	
	RunService.RenderStepped:Connect(function()
		if module.Options.Enabled then
			for player, data in pairs(tracers) do
				local line, target = unpack(data)
				if (target and (player.Team == Player.Team and module.Options.ShowTeam) or player.Team ~= Player.Team) and (target.Position - Camera.CFrame.p).Magnitude <= module.Options.MaxDistance then
					if (player == Player and module.Options.ShowSelf) or player ~= Player then
						if line.Parent ~= Screen then
							line.Parent = Screen
						end
						
						local point1 = (Camera.CFrame * CFrame.new(0, 0, -0.5) - Vector3.new(0, 3, 0)).p
						local point2 = target.Position - Vector3.new(0, 3, 0)
						
						local distance = point1 - point2
						local magnitude = distance.Magnitude
						
						local c = module.Options.Color
						
						line.CFrame = CFrame.new(point1, point2)
						
						line.A.Thickness = module.Options.Radius
						line.A.Length = magnitude
						line.A.Color3 = Color3.new(c.r*5,c.g*5,c.b*5)
						line.A.Transparency = 1 - module.Options.Opacity
					else
						line.Parent = nil
					end
				else
					line.Parent = nil
				end
			end
		end
	end)
	
	spawn(function()
		while wait(2) do
			if module.Options.Enabled then
				module:ReloadCharacters()
			end
		end
	end)
	
	return module
	
end)()
local _Aimbot = (function()
	--// Variables
	local RunService = game:GetService("RunService")
	local UserInputService = game:GetService("UserInputService")
	local Players = game:GetService("Players")
	  local Player = Players.LocalPlayer
	    local Mouse = Player:GetMouse()
	local Camera = workspace.CurrentCamera
	
	local nearestCharacters = {}
	local module = {}
	
	module.Options = {
		Easing = 2,
		Enabled = false,
		ShowTeams = false,
		MaxDistance = 500,
		Legit = false,
		AimPart = "Head",
		Onscreen = false,
		Visible = true,
		Mode = "Nearest",
		Radius = 250,
	}
	
	--// Functions
	local function findPart(Model)
		return Model:FindFirstChild(module.Options.AimPart) or Model:FindFirstChild("HumanoidRootPart") or Model.PrimaryPart or Model:FindFirstChildWhichIsA("Part", true)
	end
	
	local mousemoverel = (mousemoverel or (Input and Input.MouseMove)) or function() end
	
	local function mouseMove(x, y, depth)
		local v1, v2 = Vector2.new(x, y), Vector2.new(Mouse.X, Mouse.Y)
		local viewCenter = Vector2.new(Mouse.ViewSizeX/2, Mouse.ViewSizeY/2)
		
		if depth < 0 then
			local n = 1
			if (v1 - v2).X < 0 then
				n = -1
			end
			if math.abs(v1.X - v2.X) < Mouse.ViewSizeX * 1.5 then
				n = n / 2
			end
			v1 = v1 + Vector2.new(Mouse.ViewSizeX * n, 0)
		end
		
		local diff = (v1 - v2) / module.Options.Easing
		
		if module.Options.Legit then
			diff = diff.Unit * diff.Magnitude
		end
		
		mousemoverel(diff.X, diff.Y)
	end
	
	local function updateMouse(target)
		if not target then return end
		local posVector3 = Camera:WorldToScreenPoint(target.Position)
		local posVector2, distance = Vector2.new(posVector3.X, posVector3.Y), posVector3.Z
		mouseMove(posVector2.X, posVector2.Y, posVector3.Z)
	end
	
	local function updateNearest()
		nearestCharacters = {}
		for _, player in pairs(Players:GetPlayers()) do
			if player ~= Player then
				if (player.Team == Player.Team and module.Options.ShowTeams) or player.Team ~= Player.Team then
					if player.Character then
						local part = findPart(player.Character)
						if part then --too many ifs
							local distance = (part.Position - Camera.CFrame.p).Magnitude
							
							local a, onScreen = Camera:WorldToScreenPoint(part.Position)
							local obstructed = #Camera:GetPartsObscuringTarget({part.Position}, {player.Character, Player.Character}) > 0
							
							if distance <= module.Options.MaxDistance then
								if (module.Options.Onscreen and onScreen) or not module.Options.Onscreen then
									if (module.Options.Visible and not obstructed) or not module.Options.Visible then
										table.insert(nearestCharacters, {tostring(distance), part, a.Z})
									end
								end
							end
						end
					end
				end
			end
		end
	end
	
	local windowFocused = true
	RunService.RenderStepped:Connect(function()
		if module.Options.Enabled == false or not windowFocused then return end
		updateNearest()
		
		local dist, nearestPart = 2048
		
		table.sort(nearestCharacters, function(a, b)
			local D1, NP1 = unpack(a)
			local D2, NP2 = unpack(b)
			return tonumber(D1) < tonumber(D2)
		end)
		
		if module.Options.Mode == "Nearest" then
			if nearestCharacters[1] then
				local D, NP = unpack(nearestCharacters[1])
				nearestPart = NP
			end
		else
			for i, v in pairs(nearestCharacters) do
				local D, NP, Depth = unpack(v)
				
				local pV3 = Camera:WorldToScreenPoint(NP.Position)
				local v1, v2 = Vector2.new(pV3.X, pV3.Y), Vector2.new(Mouse.X, Mouse.Y)
				
				if (v1 - v2).Magnitude <= module.Options.Radius and Depth >= 0 then
					nearestPart = NP
					break
				end
			end
		end
		
		if nearestPart then
			updateMouse(nearestPart)
		end
	end)
	
	UserInputService.WindowFocused:Connect(function()
		windowFocused = true
	end)
	UserInputService.WindowFocusReleased:Connect(function()
		windowFocused = false
	end)
	
	return module
	
end)()
local _Flight = (function()
	--// Variables
	local RunService = game:GetService("RunService")
	local UserInputService = game:GetService("UserInputService")
	local Players = game:GetService("Players")
	  local Player = Players.LocalPlayer
	    local character = Player.Character
	local camera = workspace.CurrentCamera
	
	local module = {}
	module.Options = {
		Speed = 5,
		Smoothness = 0.2,
	}
	
	local lib, connections = {}, {}
	lib.connect = function(name, connection)
		connections[name .. tostring(math.random(1000000, 9999999))] = connection
		return connection
	end
	lib.disconnect = function(name)
		for title, connection in pairs(connections) do
			if title:find(name) == 1 then
				connection:Disconnect()
			end
		end
	end
	
	--// Functions
	local flyPart
	
	local function flyEnd()
		lib.disconnect("fly")
		if flyPart then
			--flyPart:Destroy()
		end
		character:FindFirstChildWhichIsA("Humanoid").PlatformStand = false
		if character and character.Parent and flyPart then
			for _, part in pairs(character:GetDescendants()) do
				if part:IsA("BasePart") then
					part.Velocity = Vector3.new()
				end
			end
		end
	end
	
	module.flyStart = function(enabled)
		if not enabled then flyEnd() return end
		local dir = {w = false, a = false, s = false, d = false}
		local cf = Instance.new("CFrameValue")
		
		flyPart = flyPart or Instance.new("Part")
		flyPart.Anchored = true
		pcall(function()
			flyPart.CFrame = character.HumanoidRootPart.CFrame
		end)
		
		lib.connect("fly", RunService.Heartbeat:Connect(function()
			if not character or not character.Parent or not character:FindFirstChild("HumanoidRootPart") then return end
	
			local primaryPart = character.HumanoidRootPart
			local humanoid = character:FindFirstChildWhichIsA("Humanoid")
			local speed = module.Options.Speed
			
			local x, y, z = 0, 0, 0
			if dir.w then z = -1 * speed end
			if dir.a then x = -1 * speed end
			if dir.s then z = 1 * speed end
			if dir.d then x = 1 * speed end
			if dir.q then y = 1 * speed end
			if dir.e then y = -1 * speed end
			
			flyPart.CFrame = CFrame.new(
				flyPart.CFrame.p,
				(camera.CFrame * CFrame.new(0, 0, -2048)).p
			)
			
			for _, part in pairs(character:GetChildren()) do
				if part:IsA("BasePart") then
					part.Velocity = Vector3.new()
				end
			end
			
			local moveDir = CFrame.new(x,y,z)
			cf.Value = cf.Value:lerp(moveDir, module.Options.Smoothness)
			flyPart.CFrame = flyPart.CFrame:lerp(flyPart.CFrame * cf.Value, module.Options.Smoothness)
			primaryPart.CFrame = flyPart.CFrame
			humanoid.PlatformStand = true
		end))
		lib.connect("fly", UserInputService.InputBegan:Connect(function(input, event)
			if event then return end
			local code, codes = input.KeyCode, Enum.KeyCode
			if code == codes.W then
				dir.w = true
			elseif code == codes.A then
				dir.a = true
			elseif code == codes.S then
				dir.s = true
			elseif code == codes.D then
				dir.d = true
			elseif code == codes.Q then
				dir.q = true
			elseif code == codes.E then
				dir.e = true
			elseif code == codes.Space then
				dir.q = true
			end
		end))
		lib.connect("fly", UserInputService.InputEnded:Connect(function(input, event)
			if event then return end
			local code, codes = input.KeyCode, Enum.KeyCode
			if code == codes.W then
				dir.w = false
			elseif code == codes.A then
				dir.a = false
			elseif code == codes.S then
				dir.s = false
			elseif code == codes.D then
				dir.d = false
			elseif code == codes.Q then
				dir.q = false
			elseif code == codes.E then
				dir.e = false
			elseif code == codes.Space then
				dir.q = false
			end
		end))
	end
	
	--// Events
	Player.CharacterAdded:Connect(function(char)
		character = char
	end)
	
	return module
end)()
local _Freecam = (function()
	--// Variables
	local RunService = game:GetService("RunService")
	local UserInputService = game:GetService("UserInputService")
	local Players = game:GetService("Players")
	  local Player = Players.LocalPlayer
	    local character = Player.Character
	local camera = workspace.CurrentCamera
	
	local module = {}
	module.Options = {
		Speed = 5,
		Smoothness = 0.2,
	}
	
	local lib, connections = {}, {}
	lib.connect = function(name, connection)
		connections[name .. tostring(math.random(1000000, 9999999))] = connection
		return connection
	end
	lib.disconnect = function(name)
		for title, connection in pairs(connections) do
			if title:find(name) == 1 then
				connection:Disconnect()
			end
		end
	end
	
	--// Functions
	local flyPart
	
	local function flyEnd()
		lib.disconnect("freecam")
		camera.CameraSubject = character
		pcall(function()
			character.PrimaryPart.Anchored = false
		end)
	end
	
	module.flyStart = function(enabled)
		if not enabled then flyEnd() return end
		local dir = {w = false, a = false, s = false, d = false}
		local cf = Instance.new("CFrameValue")
		local camPart = Instance.new("Part")
		camPart.Transparency = 1
		camPart.Anchored = true
		camPart.CFrame = camera.CFrame
		pcall(function()
			character.PrimaryPart.Anchored = true
		end)
		
		lib.connect("freecam", RunService.RenderStepped:Connect(function()
			local primaryPart = camPart
			camera.CameraSubject = primaryPart
			
			local speed = module.Options.Speed
			
			local x, y, z = 0, 0, 0
			if dir.w then z = -1 * speed end
			if dir.a then x = -1 * speed end
			if dir.s then z = 1 * speed end
			if dir.d then x = 1 * speed end
			if dir.q then y = 1 * speed end
			if dir.e then y = -1 * speed end
			
			primaryPart.CFrame = CFrame.new(
				primaryPart.CFrame.p,
				(camera.CFrame * CFrame.new(0, 0, -100)).p
			)
			
			local moveDir = CFrame.new(x,y,z)
			cf.Value = cf.Value:lerp(moveDir, module.Options.Smoothness)
			primaryPart.CFrame = primaryPart.CFrame:lerp(primaryPart.CFrame * cf.Value, module.Options.Smoothness)
		end))
		lib.connect("freecam", UserInputService.InputBegan:Connect(function(input, event)
			if event then return end
			local code, codes = input.KeyCode, Enum.KeyCode
			if code == codes.W then
				dir.w = true
			elseif code == codes.A then
				dir.a = true
			elseif code == codes.S then
				dir.s = true
			elseif code == codes.D then
				dir.d = true
			elseif code == codes.Q then
				dir.q = true
			elseif code == codes.E then
				dir.e = true
			elseif code == codes.Space then
				dir.q = true
			end
		end))
		lib.connect("freecam", UserInputService.InputEnded:Connect(function(input, event)
			if event then return end
			local code, codes = input.KeyCode, Enum.KeyCode
			if code == codes.W then
				dir.w = false
			elseif code == codes.A then
				dir.a = false
			elseif code == codes.S then
				dir.s = false
			elseif code == codes.D then
				dir.d = false
			elseif code == codes.Q then
				dir.q = false
			elseif code == codes.E then
				dir.e = false
			elseif code == codes.Space then
				dir.q = false
			end
		end))
	end
	
	--// Events
	Player.CharacterAdded:Connect(function(char)
		character = char
	end)
	
	return module
end)()
local _Rubberbanding = (function()
	--// Variables
	local RunService = game:GetService("RunService")
	local Players = game:GetService("Players")
	  local Player = Players.LocalPlayer
	    local Character = Player.Character
	
	local module = {}
	module.Options = {
		Enabled = false,
		Threshold = 150,
		UpdateSpeed = 100,
	}
	
	local connections = {}
	
	--// Functions
	local function getPart(Model)
		return Model.PrimaryPart or Model:FindFirstChild("HumanoidRootPart") or Model:FindFirstChildWhichIsA("Part")
	end
	
	local function connectPart(Part)
		local lastPosition = CFrame.new()
		local lastVelocity = Vector3.new()
		local lastRender = tick()
		
		connections[#connections+1] = RunService.RenderStepped:Connect(function()
			if not module.Options.Enabled then return end
			
			if Part and (tick() - lastRender >= module.Options.UpdateSpeed / 1000) then
				if (lastVelocity - Part.Velocity).Magnitude > module.Options.Threshold and Part.Velocity.Magnitude > lastVelocity.Magnitude then
					Part.Velocity = lastVelocity
					Part.CFrame = lastPosition
				end
				
				lastPosition = Part.CFrame
				lastVelocity = Part.Velocity
				lastRender = tick()
			end
		end)
	end
	
	local function onCharacter(char)
		Character = char
		for i, v in pairs(connections) do
			v:Disconnect()
			connections[i] = nil
		end
		for _, part in pairs(char:GetChildren()) do
			if part.Name == "HumanoidRootPart" then
				connectPart(part)
			end
		end
		connections[#connections+1] = Character.ChildAdded:Connect(function(child)
			if child.Name == "HumanoidRootPart" then
				connectPart(child)
			end
		end)
	end
	
	
	module.Toggle = function(enabled)
		module.Options.Enabled = enabled
		for i, v in pairs(connections) do
			v:Disconnect()
			connections[i] = nil
		end
		if enabled and Character then
			onCharacter(Character)
		end
	end
	
	--// Events
	Player.CharacterAdded:Connect(function(char)
		onCharacter(char)
	end)
	
	if Character then
		onCharacter(Character)
	end
	
	return module
	
end)()
local _AntiTP = (function()
	--// Variables
	local RunService = game:GetService("RunService")
	local Players = game:GetService("Players")
	  local Player = Players.LocalPlayer
	    local Character = Player.Character
	
	local module = {}
	module.Options = {
		Enabled = false,
		Threshold = 150,
		UpdateSpeed = 100,
	}
	
	local connections = {}
	
	--// Functions
	local function getPart(Model)
		return Model.PrimaryPart or Model:FindFirstChild("HumanoidRootPart") or Model:FindFirstChildWhichIsA("Part")
	end
	
	local function connectPart(Part)
		local lastPosition = Part.CFrame
		local lastRender = tick()
		
		connections[#connections+1] = RunService.RenderStepped:Connect(function()
			if not module.Options.Enabled then return end
			
			if Part and (tick() - lastRender >= module.Options.UpdateSpeed / 1000) then
				if (lastPosition.p - Part.Position).Magnitude > module.Options.Threshold then
					Part.CFrame = lastPosition
					Part.Velocity = Vector3.new(0, 0, 0)
				end
				
				lastPosition = Part.CFrame
				lastRender = tick()
			end
		end)
	end
	
	local function onCharacter(char)
		Character = char
		for i, v in pairs(connections) do
			v:Disconnect()
			connections[i] = nil
		end
		for _, part in pairs(char:GetChildren()) do
			if part.Name == "HumanoidRootPart" then
				connectPart(part)
			end
		end
		connections[#connections+1] = Character.ChildAdded:Connect(function(child)
			if child.Name == "HumanoidRootPart" then
				connectPart(child)
			end
		end)
	end
	
	module.Toggle = function(enabled)
		module.Options.Enabled = enabled
		for i, v in pairs(connections) do
			v:Disconnect()
			connections[i] = nil
		end
		if enabled and Character then
			onCharacter(Character)
		end
	end
	
	--// Events
	Player.CharacterAdded:Connect(function(char)
		onCharacter(char)
	end)
	
	if Character then
		onCharacter(Character)
	end
	
	return module
	
end)()
local _Noclip = (function()
	--// Variables
	local RunService = game:GetService("RunService")
	local Players = game:GetService("Players")
	  local Player = Players.LocalPlayer
	    local Character = Player.Character
	
	local module = {}
	module.Options = {
		Enabled = false,
	}
	
	local connections = {}
	
	--// Functions
	local function getPart(Model)
		return Model.PrimaryPart or Model:FindFirstChild("HumanoidRootPart") or Model:FindFirstChildWhichIsA("Part")
	end
	
	local function connectModel(Model)
		connections[#connections+1] = RunService.Stepped:Connect(function()
			if not module.Options.Enabled then return end
			for _, part in pairs(Model:GetDescendants()) do
				if part:IsA("BasePart") then
					part.CanCollide = false
				end
			end
		end)
	end
	
	module.Toggle = function(enabled)
		module.Options.Enabled = enabled
		for i, v in pairs(connections) do
			v:Disconnect()
			connections[i] = nil
		end
		if enabled and Character then
			onCharacter(Character)
		end
	end
	
	function onCharacter(char)
		for i, v in pairs(connections) do
			v:Disconnect()
			connections[i] = nil
		end
		Character = char
		connectModel(char)
	end
	
	--// Events
	Player.CharacterAdded:Connect(function(char)
		onCharacter(char)
	end)
	
	if Character then
		onCharacter(Character)
	end
	
	return module
	
end)()

--// Variables
local RunService = game:GetService("RunService")
local HttpService = game:GetService("HttpService")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
  local Player = Players.LocalPlayer
    local Mouse = Player:GetMouse()

local gui = GUIData[1]
local saveData = GUIData[2]
local screenGui = GUIData[3]

local screenscale = 250
local opacity = 1
local backcolor = Color3.new()

--// Saving
local readfile = readfile or function() end
pcall(function()
	local JSONData = readfile("OpenGui.txt")
	if JSONData then
		local LUAData = HttpService:JSONDecode(JSONData)
		saveData.Options = LUAData.Options
		saveData.Hotkeys = LUAData.Hotkeys
		print("Save Data found")
	else
		print("Save Data not found")
	end
end)


--// UI Creation

--// Render Frame
local Render = gui:create("Container", {
	Name = "Render",
})--|
	local OpenGui = Render.self:create("Toggle", {
		Name = "OpenGui",
		Default = true,
		Hotkey = tostring(Enum.KeyCode.RightControl),
		Hint = "The navigation GUI",
		Callback = function(enabled)
			for _, frame in pairs(screenGui:GetChildren()) do
				if frame:IsA("Frame") then
					frame.Visible = enabled
				end
			end
			screenGui.Modal.Visible = enabled
			screenGui.Hint.Visible = false
		end,
	})--|
		local Opacity = OpenGui.self:create("Number", {
			Name = "Opacity",
			Min = 0,
			Max = 1,
			Round = 0.01,
			Default = 0.75,
			Hint = "Transparency of the navigation GUI",
			Callback = function(alpha)
				opacity = 1 - alpha
				for _, frame in pairs(screenGui:GetChildren()) do
					if frame:IsA("Frame") then
						frame.BackgroundTransparency = 1 - alpha
						frame.OptionsFrame.BackgroundTransparency = 1 - alpha
					end
				end
			end,
		})
		local Width = OpenGui.self:create("Number", {
			Name = "Width",
			Min = 200,
			Max = 300,
			Round = 1,
			Default = 250,
			Hint = "Width of the navigation GUI",
			Callback = function(scale)
				screenscale = scale
				for _, frame in pairs(screenGui:GetChildren()) do
					if frame:IsA("Frame") then
						frame.Size = UDim2.new(0, scale, 0, frame.Size.Y.Offset)
					end
				end
			end,
		})
		local Color = OpenGui.self:create("Color", {
			Name = "Background Color",
			Default = Color3.fromRGB(40, 40, 40),
			Hint = "Background color of the navigation GUI",
			Callback = function(color)
				backcolor = color
				for _, frame in pairs(screenGui:GetChildren()) do
					if frame:IsA("Frame") then
						frame.BackgroundColor3 = color
						frame.OptionsFrame.BackgroundColor3 = color
					end
				end
			end,
		})
	local ESP = Render.self:create("Toggle", {
		Name = "ESP",
		Default = false,
		Hint = "Toggle player ESP",
		Callback = function(enabled)
			_ESP.Options.Enabled = enabled
			if enabled then
				_ESP:Enable()
				_ESP2D:Enable()
			else
				_ESP:Disable()
				_ESP2D:Disable()
			end
			_ESP2D:ReloadCharacters()
		end,
	})--|
		local ESPColor = ESP.self:create("Color", {
			Name = "ESP Color",
			Default = Color3.new(1, 1, 1),
			Hint = "Color of the player ESP",
			Callback = function(color)
				_ESP.Options.Color = color
				_ESP2D.Options.Color = color
				_ESP2D:ReloadCharacters()
			end,
		})
		local ESPShowTeam = ESP.self:create("Checkbox", {
			Name = "Show Team",
			Default = false,
			Hint = "Players on your team are highlighted",
			Callback = function(enabled)
				_ESP.Options.ShowTeam = enabled
				_ESP2D.Options.ShowTeam = enabled
				_ESP2D:ReloadCharacters()
			end,
		})
		local ESPShowSelf = ESP.self:create("Checkbox", {
			Name = "Show Self",
			Default = false,
			Hint = "Include yourself in the ESP",
			Callback = function(enabled)
				_ESP.Options.ShowSelf = enabled
				_ESP2D.Options.ShowSelf = enabled
				_ESP2D:ReloadCharacters()
			end,
		})
		local ESPTeamColor = ESP.self:create("Checkbox", {
			Name = "Team Color",
			Default = false,
			Hint = "The ESP's color corresponds to the player's team",
			Callback = function(enabled)
				_ESP.Options.TeamColor = enabled
				_ESP2D.Options.TeamColor = enabled
				_ESP2D:ReloadCharacters()
			end,
		})
		local ESPShowDescendants = ESP.self:create("Checkbox", {
			Name = "Show Descendants",
			Default = false,
			Hint = "Highlight items like accessories",
			Callback = function(enabled)
				_ESP.Options.ShowDescendants = enabled
				_ESP2D.Options.ShowDescendants = enabled
				_ESP2D:ReloadCharacters()
			end,
		})
		local ESPDirection = ESP.self:create("Checkbox", {
			Name = "Show Direction",
			Default = false,
			Hint = "Show where the player is facing",
			Callback = function(enabled)
				_ESP.Options.Arrow = enabled
				_ESP2D.Options.Arrow = enabled
				_ESP2D:ReloadCharacters()
			end,
		})
		local ESPOpacity = ESP.self:create("Number", {
			Name = "Opacity",
			Default = 0.5,
			Min = 0,
			Max = 1,
			Round = 0.01,
			Hint = "Visibility of the ESP",
			Callback = function(opacity)
				_ESP.Options.Opacity = opacity
				_ESP2D.Options.Opacity = opacity
				_ESP2D:ReloadCharacters()
			end,
		})
		local ESPMaxDistance = ESP.self:create("Number", {
			Name = "Max Distance",
			Default = 500,
			Min = 32,
			Max = 2048,
			Round = 0.5,
			Hint = "The maximum distance of the ESP",
			Callback = function(distance)
				_ESP.Options.MaxDistance = distance
				_ESP2D.Options.MaxDistance = distance
				_ESP2D:ReloadCharacters()
			end,
		})
		local ESPMode = ESP.self:create("Mode", {
			Name = "ESP Mode",
			Default = 1,
			Modes = {"Shader", "Default", "Box", "Square"},
			Hint = "The type of ESP used",
			Callback = function(mode)
				_ESP.Options.Mode = mode
				_ESP2D.Options.Mode = mode
				_ESP:ReloadCharacters()
				_ESP2D:ReloadCharacters()
			end,
		})
	local Chams = Render.self:create("Toggle", {
		Name = "Chams",
		Default = false,
		Hint = "Render players through walls",
		Callback = function(enabled)
			_Chams.Options.Enabled = enabled
			if enabled then
				_Chams:Enable()
			else
				_Chams:Disable()
			end
		end,
	})--|
		local ChamsColor = Chams.self:create("Color", {
			Name = "Chams Color",
			Default = Color3.new(1, 1, 1),
			Hint = "Color of the player chams",
			Callback = function(color)
				_Chams.Options.Color = color
			end,
		})
		local ChamsShowTeam = Chams.self:create("Checkbox", {
			Name = "Show Team",
			Default = false,
			Hint = "Include your teammates",
			Callback = function(enabled)
				_Chams.Options.ShowTeam = enabled
			end,
		})
		local ChamsShowSelf = Chams.self:create("Checkbox", {
			Name = "Show Self",
			Default = false,
			Hint = "Include yourself",
			Callback = function(enabled)
				_Chams.Options.ShowSelf = enabled
			end,
		})
		local ChamsTeamColor = Chams.self:create("Checkbox", {
			Name = "Team Color",
			Default = false,
			Hint = "The chams color corresponds to the player's team",
			Callback = function(enabled)
				_Chams.Options.TeamColor = enabled
			end,
		})
		local ChamsShowDescendants = Chams.self:create("Checkbox", {
			Name = "Show Descendants",
			Default = false,
			Hint = "Highlight items like accessories",
			Callback = function(enabled)
				_Chams.Options.ShowDescendants = enabled
			end,
		})
		local ChamsMode = Chams.self:create("Mode", {
			Name = "Chams Mode",
			Default = 1,
			Modes = {"Opaque", "Shader"},
			Hint = "The type of chams used",
			Callback = function(mode)
				_Chams.Options.Mode = mode
				_Chams:ReloadCharacters()
			end,
		})
		local ChamsOpacity = Chams.self:create("Number", {
			Name = "Opacity",
			Default = 0.5,
			Min = 0,
			Max = 1,
			Round = 0.01,
			Hint = "Visibility of the chams",
			Callback = function(opacity)
				_Chams.Options.Opacity = opacity
			end,
		})
		local ChamsMaxDistance = Chams.self:create("Number", {
			Name = "Max Distance",
			Default = 500,
			Min = 32,
			Max = 2048,
			Round = 0.5,
			Hint = "The chams' maximum distance",
			Callback = function(distance)
				_Chams.Options.MaxDistance = distance
			end,
		})
	local Tracers = Render.self:create("Toggle", {
		Name = "Tracers",
		Default = false,
		Hint = "Draw lines to other players",
		Callback = function(enabled)
			_Tracers.Options.Enabled = enabled
			if enabled then
				_Tracers:Enable()
			else
				_Tracers:Disable()
			end
		end,
	})--|
		local TracersColor = Tracers.self:create("Color", {
			Name = "Tracers Color",
			Default = Color3.new(1, 1, 1),
			Hint = "Color of the tracers",
			Callback = function(color)
				_Tracers.Options.Color = color
			end,
		})
		local TracersShowTeam = Tracers.self:create("Checkbox", {
			Name = "Show Team",
			Default = false,
			Hint = "Include your teammates",
			Callback = function(enabled)
				_Tracers.Options.ShowTeam = enabled
				_Tracers:ReloadCharacters()
			end,
		})
		local TracersShowSelf = Tracers.self:create("Checkbox", {
			Name = "Show Self",
			Default = false,
			Hint = "Include yourself",
			Callback = function(enabled)
				_Tracers.Options.ShowSelf = enabled
				_Tracers:ReloadCharacters()
			end,
		})
		local TracersTeamColor = Tracers.self:create("Checkbox", {
			Name = "Team Color",
			Default = false,
			Hint = "Tracer colors correspond to the player's team",
			Callback = function(enabled)
				_Tracers.Options.TeamColor = enabled
			end,
		})
		local TracersOpacity = Tracers.self:create("Number", {
			Name = "Opacity",
			Default = 1,
			Min = 0,
			Max = 1,
			Round = 0.01,
			Hint = "Visibility of the tracers",
			Callback = function(opacity)
				_Tracers.Options.Opacity = opacity
			end,
		})
		local TracersMaxDistance = Tracers.self:create("Number", {
			Name = "Max Distance",
			Default = 500,
			Min = 32,
			Max = 2048,
			Round = 0.5,
			Hint = "The maximum distance in which tracers are drawn",
			Callback = function(distance)
				_Tracers.Options.MaxDistance = distance
			end,
		})
		local TracersWidth = Tracers.self:create("Number", {
			Name = "Width",
			Default = 2,
			Min = 1,
			Max = 10,
			Round = 1,
			Hint = "Width of the tracers",
			Callback = function(value)
				_Tracers.Options.Radius = value
			end,
		})
	local Freecam = Render.self:create("Toggle", {
		Name = "Freecam",
		Default = false,
		Hint = "Move your camera freely",
		Callback = function(enabled)
			_Freecam.flyStart(enabled)
		end,
	})--|
		local FreecamSpeed = Freecam.self:create("Number", {
			Name = "Speed",
			Default = 5,
			Min = 0.1,
			Max = 100,
			Round = 0.1,
			Hint = "Camera speed",
			Callback = function(value)
				_Freecam.Options.Speed = value
			end,
		})
		local FreecamSpeed = Freecam.self:create("Number", {
			Name = "Smoothness",
			Default = 0.2,
			Min = 0.1,
			Max = 1,
			Round = 0.01,
			Hint = "Smoothness of the interpolation",
			Callback = function(value)
				_Freecam.Options.Smoothness = value
			end,
		})

--// Combat Frame
local Combat = gui:create("Container", {
	Name = "Combat",
})--|
	local Aimbot = Combat.self:create("Toggle", {
		Name = "Aimbot",
		Default = false,
		Hint = "Automatically point to other players, hotkey recommended",
		Callback = function(enabled)
			_Aimbot.Options.Enabled = enabled
		end,
	})--]
		local AimbotEasing = Aimbot.self:create("Number", {
			Name = "Easing",
			Default = 2,
			Min = 1.3,
			Max = 32,
			Round = 0.1,
			Hint = "Smoothness of the aimbot",
			Callback = function(value)
				_Aimbot.Options.Easing = value
			end,
		})
		local AimbotLegit = Aimbot.self:create("Checkbox", {
			Name = "Legit",
			Hint = "Give the aimbot a maximum speed, looks more legit",
			Callback = function(value)
				_Aimbot.Options.Legit = value
			end,
		})
		local AimbotMaxDistance = Aimbot.self:create("Number", {
			Name = "Max Distance",
			Default = 500,
			Min = 32,
			Max = 2048,
			Round = 1,
			Hint = "The aimbot's maximum distance",
			Callback = function(value)
				_Aimbot.Options.MaxDistance = value
			end,
		})
		local AimbotMode = Aimbot.self:create("Mode", {
			Name = "Aim Target",
			Modes = {
				"Head",
				"Torso",
			},
			Hint = "Where the aimbot will aim",
			Callback = function(value)
				_Aimbot.Options.AimPart = value
			end,
		})
		local AimbotShowTeam = Aimbot.self:create("Checkbox", {
			Name = "Target Team",
			Hint = "Target your teammates",
			Callback = function(value)
				_Aimbot.Options.ShowTeams = value
			end,
		})
		local AimbotOnscreen = Aimbot.self:create("Checkbox", {
			Name = "Target On-Screen",
			Hint = "Target players only in front of you",
			Default = false,
			Callback = function(value)
				_Aimbot.Options.Onscreen = value
			end,
		})
		local AimbotVisible = Aimbot.self:create("Checkbox", {
			Name = "Target Visible",
			Hint = "Ignore players obstructed from view",
			Default = false,
			Callback = function(value)
				_Aimbot.Options.Visible = value
			end,
		})
		local AimbotMode = Aimbot.self:create("Mode", {
			Name = "Aimbot Mode",
			Hint = "Change who the aimbot targets",
			Default = 1,
			Modes = {
				"Nearest",
				"Snap",
			},
			Callback = function(value)
				_Aimbot.Options.Mode = value
			end,
		})--]
			local AimbotModeRadius = AimbotMode.self:create("Number", {
				Name = "Snap Radius",
				Default = 250,
				Min = 32,
				Max = 1024,
				Round = 1,
				Hint = "The detection radius of the aimbot mode 'Snap'",
				Callback = function(value)
					_Aimbot.Options.Radius = value
				end,
			})

--// Movement
local Movement = gui:create("Container", {
	Name = "Movement",
})--|
	local Flight = Movement.self:create("Toggle", {
		Name = "Flight",
		Default = false,
		Hint = "Toggle player flight (uses CFrame)",
		Callback = function(enabled)
			_Flight.flyStart(enabled)
		end,
	})--|
		local FlightSpeed = Flight.self:create("Number", {
			Name = "Speed",
			Default = 5,
			Min = 0.1,
			Max = 100,
			Round = 0.1,
			Hint = "Flight speed",
			Callback = function(value)
				_Flight.Options.Speed = value
			end,
		})
		local FlightSpeed = Flight.self:create("Number", {
			Name = "Smoothness",
			Default = 0.2,
			Min = 0.1,
			Max = 1,
			Round = 0.01,
			Hint = "Smoothness of the interpolation",
			Callback = function(value)
				_Flight.Options.Smoothness = value
			end,
		})

--// Player
local PlayerTab = gui:create("Container", {
	Name = "Player",
})--|
	local Rubberbanding = PlayerTab.self:create("Toggle", {
		Name = "Rubberbanding",
		Default = false,
		Hint = "Get set back if your velocity changes above the threshold",
		Callback = function(enabled)
			_Rubberbanding.Toggle(enabled)
		end,
	})--|
		local RubberbandingThreshold = Rubberbanding.self:create("Number", {
			Name = "Threshold",
			Default = false,
			Min = 50,
			Max = 1000,
			Default = 150,
			Round = 1,
			Hint = "Threshold for magnitude check",
			Callback = function(value)
				_Rubberbanding.Options.Threshold = value
			end,
		})
		local RubberbandingSpeed = Rubberbanding.self:create("Number", {
			Name = "Update Speed",
			Default = false,
			Min = 1,
			Max = 500,
			Default = 100,
			Round = 1,
			Hint = "How often it checks the velocity in ms",
			Callback = function(value)
				_Rubberbanding.Options.UpdateSpeed = value
			end,
		})
	local AntiTP = PlayerTab.self:create("Toggle", {
		Name = "Anti TP",
		Default = false,
		Hint = "Prevent teleporting large distances",
		Callback = function(enabled)
			_AntiTP.Toggle(enabled)
		end,
	})--|
		local AntiTPThreshold = AntiTP.self:create("Number", {
			Name = "Threshold",
			Min = 1,
			Max = 1000,
			Default = 150,
			Round = 1,
			Hint = "Maximum distance",
			Callback = function(value)
				_AntiTP.Options.Threshold = value
			end,
		})
		local AntiTPSpeed = AntiTP.self:create("Number", {
			Name = "Update Speed",
			Min = 1,
			Max = 500,
			Default = 100,
			Round = 1,
			Hint = "How often it checks the position in ms",
			Callback = function(value)
				_AntiTP.Options.UpdateSpeed = value
			end,
		})
	local Noclip = PlayerTab.self:create("Toggle", {
		Name = "No Collision",
		Default = false,
		Hint = "Ignore object collision",
		Callback = function(enabled)
			_Noclip.Toggle(enabled)
		end,
	})

--// UI Functionality
RunService.RenderStepped:Connect(function()
	for _, frame in pairs(screenGui:GetChildren()) do
		if frame:IsA("Frame") then
			frame.Size = UDim2.new(0, screenscale, 0, frame.Size.Y.Offset)
			
			frame.BackgroundTransparency = opacity
			frame.OptionsFrame.BackgroundTransparency = opacity
			
			frame.BackgroundColor3 = backcolor
			frame.OptionsFrame.BackgroundColor3 = backcolor
		end
	end
end)
