local lib = {}

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local uis = game:GetService("UserInputService")

local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()

local function round(num, bracket)
	bracket = bracket or 1
	local a = math.floor(num/bracket + (math.sign(num) * 0.5)) * bracket
	if a < 0 then
		a = a + bracket
	end
	return a
end

function lib:Init()
	local Universal = Instance.new("ScreenGui")
	local tabContainerBody = Instance.new("Frame")
	local tabContainer = Instance.new("Frame")
	local tabFLayout = Instance.new("UIListLayout")
	
	local tabLayout = Instance.new("UIListLayout")
	
	Universal.Name = "universal.fun"
	Universal.Parent = game:GetService("CoreGui")
	
	uis.InputBegan:Connect(function(key, focused)
		if not focused and key.KeyCode == Enum.KeyCode.RightShift then
			Universal.Enabled = not Universal.Enabled
		end
	end)

	tabContainerBody.Name = "tabContainerBody"
	tabContainerBody.Parent = Universal
	tabContainerBody.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
	tabContainerBody.BorderSizePixel = 0
	tabContainerBody.Position = UDim2.new(0.012, 0, 0.021, 0)
	tabContainerBody.Size = UDim2.new(0, 37, 0, 37)
	
	tabContainer.Name = "tabContainer"
	tabContainer.Parent = Universal
	tabContainer.BackgroundColor3 = Color3.fromRGB(153, 153, 153)
	tabContainer.BackgroundTransparency = 1.000
	tabContainer.Position = UDim2.new(0.0543293729, 0, 0.0213464703, 0)
	tabContainer.Size = UDim2.new(0, 147, 0, 596)

	tabFLayout.Name = "tabFLayout"
	tabFLayout.Parent = tabContainer
	tabFLayout.SortOrder = Enum.SortOrder.LayoutOrder
	
	tabLayout.Name = "tabLayout"
	tabLayout.Parent = tabContainerBody
	tabLayout.SortOrder = Enum.SortOrder.LayoutOrder
	
	local tab = {}
	
	function tab:CreateTab(title, icon)
		icon = icon or "rbxassetid://4034483344"
		
		local tabBtnBG = Instance.new("Frame")
		local tabButton = Instance.new("ImageButton")
		local tabBtnToolTip = Instance.new("TextLabel")
		local tabFrame = Instance.new("Frame")
		local elementLayout = Instance.new("UIListLayout")
		local Spacing = Instance.new("TextLabel")
		
		tabBtnBG.Name = "tabBtnBG"
		tabBtnBG.Parent = tabContainerBody
		tabBtnBG.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
		tabBtnBG.BorderSizePixel = 0
		tabBtnBG.Size = UDim2.new(0, 37, 0, 37)

		tabButton.Name = "tabButton"
		tabButton.Parent = tabBtnBG
		tabButton.AnchorPoint = Vector2.new(0.5, 0.5)
		tabButton.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
		tabButton.BackgroundTransparency = 1.000
		tabButton.BorderColor3 = Color3.fromRGB(27, 42, 53)
		tabButton.BorderSizePixel = 0
		tabButton.Position = UDim2.new(0.5, 0, 0.5, 0)
		tabButton.Size = UDim2.new(0, 20, 0, 20)
		tabButton.Image = icon
		tabButton.MouseButton1Click:Connect(function()
			for i, v in pairs(tabContainer:GetChildren()) do
				if v:IsA("Frame") then
					v.Visible = false
				end
			end
			for _,c in pairs(Universal.tabContainerBody:GetDescendants()) do
				if c.Name == "tabButton" then
					c.ImageColor3 = Color3.fromRGB(255, 255, 255)
					c.Parent.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
				end
			end
			tabFrame.Visible = true
			tabButton.ImageColor3 = Color3.fromRGB(15, 15, 15)
			tabBtnBG.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
		end)
		
		tabBtnToolTip.Name = "tabBtnToolTip"
		tabBtnToolTip.Parent = tabButton
		tabBtnToolTip.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
		tabBtnToolTip.BorderSizePixel = 0
		tabBtnToolTip.Position = UDim2.new(1.75, 0, 0.0500000007, 0)
		tabBtnToolTip.Size = UDim2.new(0, 0, 0, 19) -- 54
		tabBtnToolTip.Font = Enum.Font.Gotham
		tabBtnToolTip.Text = title
		tabBtnToolTip.TextColor3 = Color3.fromRGB(15, 15, 15)
		tabBtnToolTip.TextSize = 14.000
		tabBtnToolTip.ZIndex = 500
		tabBtnToolTip.ClipsDescendants = true
		
		tabBtnBG.MouseEnter:Connect(function()
			local TI = TweenInfo.new(0.16, Enum.EasingStyle.Sine, Enum.EasingDirection.In)
			
			TweenService:Create(tabBtnToolTip, TI, {Size = UDim2.new(0, 48, 0, 19)}):Play()
		end)
		
		tabBtnBG.MouseLeave:Connect(function()
			local TI = TweenInfo.new(0.16, Enum.EasingStyle.Sine, Enum.EasingDirection.In)

			TweenService:Create(tabBtnToolTip, TI, {Size = UDim2.new(0, 0, 0, 19)}):Play()
		end)

		tabFrame.Name = title.."_tabframe"
		tabFrame.Parent = tabContainer
		tabFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
		tabFrame.BorderSizePixel = 0
		tabFrame.AnchorPoint = Vector2.new(0.5, 0.5)
		tabFrame.Position = UDim2.new(0.117, 0, 0.054, 0)
		tabFrame.Size = UDim2.new(0, 147, 0, 6)
		tabFrame.Visible = false
		
		local function Resize(num)
			tabFrame.Size = tabFrame.Size + UDim2.new(0, 0, 0, num)
		end

		elementLayout.Name = "elementLayout"
		elementLayout.Parent = tabFrame
		elementLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
		elementLayout.SortOrder = Enum.SortOrder.LayoutOrder
		elementLayout.Padding = UDim.new(0, 2)

		Spacing.Parent = tabFrame
		Spacing.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
		Spacing.BackgroundTransparency = 1.000
		Spacing.BorderSizePixel = 0
		Spacing.Position = UDim2.new(0, 0, 0.235294119, 0)
		Spacing.Size = UDim2.new(0, 147, 0, 3)
		Spacing.Font = Enum.Font.SourceSans
		Spacing.Text = ""
		Spacing.TextColor3 = Color3.fromRGB(0, 0, 0)
		Spacing.TextSize = 14.000
		
		local elements = {}
		
		function elements:CreateButton(text, callback)
			text = text or "Button"
			callback = callback or function() end
			
			local txtButton = Instance.new("TextButton")
			local Touch = Instance.new("ImageLabel")

			txtButton.Name = "txtButton"
			txtButton.Parent = tabFrame
			txtButton.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
			txtButton.BorderSizePixel = 0
			txtButton.Size = UDim2.new(0, 134, 0, 30)
			txtButton.AutoButtonColor = false
			txtButton.Font = Enum.Font.Gotham
			txtButton.Text = "  " .. text
			txtButton.TextColor3 = Color3.fromRGB(255, 255, 255)
			txtButton.TextSize = 14.000
			txtButton.TextXAlignment = Enum.TextXAlignment.Left
			txtButton.MouseButton1Click:Connect(function()
				callback()
			end)

			Touch.Parent = txtButton
			Touch.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			Touch.BackgroundTransparency = 1.000
			Touch.BorderSizePixel = 0
			Touch.Position = UDim2.new(0.810000002, 0, 0.159999996, 0)
			Touch.Size = UDim2.new(0, 21, 0, 21)
			Touch.Image = "rbxassetid://3926305904"
			Touch.ImageRectOffset = Vector2.new(84, 204)
			Touch.ImageRectSize = Vector2.new(42, 42)
			
			Resize(33)
		end
		
		function elements:CreateToggle(text, callback)
			text = text or "Toggle"
			callback = callback or function() end
			
			local toggleFrame = Instance.new("TextLabel")
			local toggleButton = Instance.new("TextButton")
			
			local State = false

			toggleFrame.Name = "toggleFrame"
			toggleFrame.Parent = tabFrame
			toggleFrame.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
			toggleFrame.BorderSizePixel = 0
			toggleFrame.Size = UDim2.new(0, 134, 0, 30)
			toggleFrame.Font = Enum.Font.Gotham
			toggleFrame.Text = "  " .. text
			toggleFrame.TextColor3 = Color3.fromRGB(255, 255, 255)
			toggleFrame.TextSize = 14.000
			toggleFrame.TextXAlignment = Enum.TextXAlignment.Left

			toggleButton.Name = "toggleButton"
			toggleButton.Parent = toggleFrame
			toggleButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
			toggleButton.BorderColor3 = Color3.fromRGB(60, 60, 60)
			toggleButton.Position = UDim2.new(0.828, 0, 0.233, 0)
			toggleButton.Size = UDim2.new(0, 15, 0, 15)
			toggleButton.AutoButtonColor = false
			toggleButton.Font = Enum.Font.SourceSans
			toggleButton.Text = ""
			toggleButton.TextColor3 = Color3.fromRGB(0, 0, 0)
			toggleButton.TextSize = 14.000
			toggleButton.MouseButton1Click:Connect(function()
				State = not State
				if State then
					toggleButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
					callback(State)
				else
					toggleButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
					callback(State)
				end
			end)
			
			Resize(33)
		end
		
		function elements:CreateBind(text, key, callback)
			text = text or "Bind"
			key = key or Enum.KeyCode.Q
			callback = callback or function() end
			
			local blacklisted = {
				Return = true,
				Space = true,
				Tab = true,
				W = true,
				A = true,
				S = true,
				D = true,
				I = true,
				O = true,
				Unknown = true
			}

			local short = {
				RightControl = "RCtrl",
				LeftControl = "LCtrl",
				LeftShift = "LShift",
				RightShift = "RShift",
				MouseButton1 = "M1",
				MouseButton2 = "M2",
				LeftAlt = "LAlt",
				RightAlt = "RAlt"
			}

			local oldKey = key.Name
			
			local keybindFrame = Instance.new("TextLabel")
			local keybindButton = Instance.new("TextButton")

			keybindFrame.Name = "keybindFrame"
			keybindFrame.Parent = tabFrame
			keybindFrame.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
			keybindFrame.BorderSizePixel = 0
			keybindFrame.Size = UDim2.new(0, 134, 0, 30)
			keybindFrame.Font = Enum.Font.Gotham
			keybindFrame.Text = "  " .. text
			keybindFrame.TextColor3 = Color3.fromRGB(255, 255, 255)
			keybindFrame.TextSize = 14.000
			keybindFrame.TextXAlignment = Enum.TextXAlignment.Left

			keybindButton.Name = "keybindButton"
			keybindButton.Parent = keybindFrame
			keybindButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
			keybindButton.BorderColor3 = Color3.fromRGB(60, 60, 60)
			keybindButton.Position = UDim2.new(0.716417909, 0, 0.233333334, 0)
			keybindButton.Size = UDim2.new(0, 30, 0, 15)
			keybindButton.AutoButtonColor = false
			keybindButton.Font = Enum.Font.Gotham
			keybindButton.Text = oldKey
			keybindButton.TextColor3 = Color3.fromRGB(255, 255, 255)
			keybindButton.TextSize = 14.000
			keybindButton.MouseButton1Click:Connect(function()
				keybindButton.Text = ". . ."
				local inputbegan = game:GetService("UserInputService").InputBegan:wait()
				if not blacklisted[inputbegan.KeyCode.Name] then
					keybindButton.Text = short[inputbegan.KeyCode.Name] or inputbegan.KeyCode.Name
					oldKey = inputbegan.KeyCode.Name
				else
					keybindButton.Text = short[oldKey] or oldKey
				end
			end)
			
			game:GetService("UserInputService").InputBegan:connect(function(keyPressed, focused)
				if not focused then
					if keyPressed.KeyCode.Name == oldKey then
						callback(oldKey)
					end
				end
			end)
			
			Resize(33)
		end
		
		function elements:CreateColorpicker(text, preset, callback)
			text = text or "Colorpicker"
			preset = preset or Color3.fromRGB(255, 0, 0)
			callback = callback or function() end
			
			local ColorPickerToggled = false
			local OldToggleColor = Color3.fromRGB(0, 0, 0)
			local OldColor = Color3.fromRGB(0, 0, 0)
			local OldColorSelectionPosition = nil
			local OldHueSelectionPosition = nil
			local ColorH, ColorS, ColorV = 1, 1, 1
			local RainbowColorPicker = false
			local ColorPickerInput = nil
			local ColorInput = nil
			local HueInput = nil
			
			local cPickerLabel = Instance.new("TextLabel")
			local cPickerBtn = Instance.new("TextButton")
			local cPickerFrame = Instance.new("Frame")
			local Color = Instance.new("ImageButton")
			local ColorSelection = Instance.new("ImageLabel")
			local Hue = Instance.new("ImageButton")
			local HueGradient = Instance.new("UIGradient")
			local HueSelection = Instance.new("ImageLabel")
			local doneBtn = Instance.new("TextButton")
			local doneTouch = Instance.new("ImageLabel")

			cPickerLabel.Name = "cPickerLabel"
			cPickerLabel.Parent = tabFrame
			cPickerLabel.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
			cPickerLabel.BorderSizePixel = 0
			cPickerLabel.Size = UDim2.new(0, 134, 0, 30)
			cPickerLabel.Font = Enum.Font.Gotham
			cPickerLabel.Text = "  Colorpicker"
			cPickerLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
			cPickerLabel.TextSize = 14.000
			cPickerLabel.TextXAlignment = Enum.TextXAlignment.Left

			cPickerBtn.Name = "cPickerBtn"
			cPickerBtn.Parent = cPickerLabel
			cPickerBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
			cPickerBtn.BorderColor3 = Color3.fromRGB(60, 60, 60)
			cPickerBtn.Position = UDim2.new(0.832388103, 0, 0.233333334, 0)
			cPickerBtn.Size = UDim2.new(0, 15, 0, 15)
			cPickerBtn.AutoButtonColor = false
			cPickerBtn.Font = Enum.Font.Gotham
			cPickerBtn.Text = ""
			cPickerBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
			cPickerBtn.TextSize = 14.000
			
			cPickerFrame.Name = "cPickerFrame"
			cPickerFrame.Parent = cPickerLabel
			cPickerFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
			cPickerFrame.BorderSizePixel = 0
			cPickerFrame.Position = UDim2.new(1.119403, 0, 0, 0)
			cPickerFrame.Size = UDim2.new(0, 171, 0, 162)
			cPickerFrame.Visible = false

			Color.Name = "Color"
			Color.Parent = cPickerFrame
			Color.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
			Color.BorderSizePixel = 0
			Color.Position = UDim2.new(0.0529520139, 0, 0.0479452051, 0)
			Color.Size = UDim2.new(0, 125, 0, 108)
			Color.AutoButtonColor = false
			Color.Image = "rbxassetid://4155801252"

			ColorSelection.Name = "ColorSelection"
			ColorSelection.Parent = Color
			ColorSelection.AnchorPoint = Vector2.new(0.5, 0.5)
			ColorSelection.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			ColorSelection.BackgroundTransparency = 1.000
			ColorSelection.BorderSizePixel = 0
			ColorSelection.Position = UDim2.new(preset and select(3, Color3.toHSV(preset)))
			ColorSelection.Size = UDim2.new(0, 12, 0, 12)
			ColorSelection.Image = "http://www.roblox.com/asset/?id=4805639000"

			Hue.Name = "Hue"
			Hue.Parent = cPickerFrame
			Hue.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			Hue.BorderSizePixel = 0
			Hue.Position = UDim2.new(0.830729902, 0, 0.0479452051, 0)
			Hue.Size = UDim2.new(0, 21, 0, 108)
			Hue.AutoButtonColor = false

			HueGradient.Color = ColorSequence.new{ColorSequenceKeypoint.new(0.00, Color3.fromRGB(255, 0, 4)), ColorSequenceKeypoint.new(0.20, Color3.fromRGB(234, 255, 0)), ColorSequenceKeypoint.new(0.40, Color3.fromRGB(21, 255, 0)), ColorSequenceKeypoint.new(0.60, Color3.fromRGB(0, 255, 255)), ColorSequenceKeypoint.new(0.80, Color3.fromRGB(0, 17, 255)), ColorSequenceKeypoint.new(0.90, Color3.fromRGB(255, 0, 251)), ColorSequenceKeypoint.new(1.00, Color3.fromRGB(255, 0, 4))}
			HueGradient.Rotation = 270
			HueGradient.Name = "HueGradient"
			HueGradient.Parent = Hue

			HueSelection.Name = "HueSelection"
			HueSelection.Parent = Hue
			HueSelection.AnchorPoint = Vector2.new(0.5, 0.5)
			HueSelection.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			HueSelection.BackgroundTransparency = 1.000
			HueSelection.BorderSizePixel = 0
			HueSelection.Position = UDim2.new(0.48, 0, 1 - select(1, Color3.toHSV(preset)))
			HueSelection.Size = UDim2.new(0, 12, 0, 12)
			HueSelection.Image = "http://www.roblox.com/asset/?id=4805639000"
			
			cPickerBtn.MouseButton1Click:Connect(function()
				ColorPickerToggled = not ColorPickerToggled
				for i,v in pairs(tabFrame:GetDescendants()) do
					if v.Name == "cPickerFrame" then
						v.Visible = false
					elseif v.Name == "dpListContainer" then
						v.Visible = false
					end
				end
				cPickerFrame.Visible = ColorPickerToggled
			end)

			local function UpdateColorPicker(nope)
				cPickerBtn.BackgroundColor3 = Color3.fromHSV(ColorH, ColorS, ColorV)
				Color.BackgroundColor3 = Color3.fromHSV(ColorH, 1, 1)

				callback(cPickerBtn.BackgroundColor3)
			end

			ColorH =
				1 -
				(math.clamp(HueSelection.AbsolutePosition.Y - Hue.AbsolutePosition.Y, 0, Hue.AbsoluteSize.Y) /
					Hue.AbsoluteSize.Y)
			ColorS =
				(math.clamp(ColorSelection.AbsolutePosition.X - Color.AbsolutePosition.X, 0, Color.AbsoluteSize.X) /
					Color.AbsoluteSize.X)
			ColorV =
				1 -
				(math.clamp(ColorSelection.AbsolutePosition.Y - Color.AbsolutePosition.Y, 0, Color.AbsoluteSize.Y) /
					Color.AbsoluteSize.Y)

			cPickerBtn.BackgroundColor3 = preset
			Color.BackgroundColor3 = preset
			callback(cPickerBtn.BackgroundColor3)

			Color.InputBegan:Connect(
				function(input)
					if input.UserInputType == Enum.UserInputType.MouseButton1 then
						if RainbowColorPicker then
							return
						end

						if ColorInput then
							ColorInput:Disconnect()
						end

						ColorInput =
							RunService.RenderStepped:Connect(
								function()
								local ColorX =
									(math.clamp(Mouse.X - Color.AbsolutePosition.X, 0, Color.AbsoluteSize.X) /
										Color.AbsoluteSize.X)
								local ColorY =
									(math.clamp(Mouse.Y - Color.AbsolutePosition.Y, 0, Color.AbsoluteSize.Y) /
										Color.AbsoluteSize.Y)

								ColorSelection.Position = UDim2.new(ColorX, 0, ColorY, 0)
								ColorS = ColorX
								ColorV = 1 - ColorY

								UpdateColorPicker(true)
							end
							)
					end
				end
			)

			Color.InputEnded:Connect(
				function(input)
					if input.UserInputType == Enum.UserInputType.MouseButton1 then
						if ColorInput then
							ColorInput:Disconnect()
						end
					end
				end
			)

			Hue.InputBegan:Connect(
				function(input)
					if input.UserInputType == Enum.UserInputType.MouseButton1 then
						if RainbowColorPicker then
							return
						end

						if HueInput then
							HueInput:Disconnect()
						end

						HueInput =
							RunService.RenderStepped:Connect(
								function()
								local HueY =
									(math.clamp(Mouse.Y - Hue.AbsolutePosition.Y, 0, Hue.AbsoluteSize.Y) /
										Hue.AbsoluteSize.Y)

								HueSelection.Position = UDim2.new(0.48, 0, HueY, 0)
								ColorH = 1 - HueY

								UpdateColorPicker(true)
							end
							)
					end
				end
			)

			Hue.InputEnded:Connect(
				function(input)
					if input.UserInputType == Enum.UserInputType.MouseButton1 then
						if HueInput then
							HueInput:Disconnect()
						end
					end
				end
			)
			
			doneBtn.Name = "doneBtn"
			doneBtn.Parent = cPickerFrame
			doneBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
			doneBtn.BorderSizePixel = 0
			doneBtn.Position = UDim2.new(0.0492899045, 0, 0.756858528, 0)
			doneBtn.Size = UDim2.new(0, 154, 0, 30)
			doneBtn.AutoButtonColor = false
			doneBtn.Font = Enum.Font.Gotham
			doneBtn.Text = "  Done"
			doneBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
			doneBtn.TextSize = 14.000
			doneBtn.TextXAlignment = Enum.TextXAlignment.Left
			doneBtn.MouseButton1Click:Connect(function()
				ColorPickerToggled = false
				cPickerFrame.Visible = false
				callback(cPickerBtn.BackgroundColor3)
			end)
			
			doneTouch.Name = "doneTouch"
			doneTouch.Parent = doneBtn
			doneTouch.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			doneTouch.BackgroundTransparency = 1.000
			doneTouch.BorderSizePixel = 0
			doneTouch.Position = UDim2.new(0.829999983, 0, 0.180000007, 0)
			doneTouch.Size = UDim2.new(0, 21, 0, 21)
			doneTouch.Image = "rbxassetid://3926305904"
			doneTouch.ImageRectOffset = Vector2.new(84, 204)
			doneTouch.ImageRectSize = Vector2.new(42, 42)
			
			Resize(33)
		end
		
		function elements:CreateSlider(text, default, min, max, float, callback)
			local sliderFunc = {}
			text = text or "Slider"
			default = default or 0
			min = min or 0
			max = max or 100
			callback = callback or function() end
			
			callback(default)
			
			local sliderFrame = Instance.new("Frame")
			local sliderText = Instance.new("TextLabel")
			local sliderValue = Instance.new("TextLabel")
			local sliderButton = Instance.new("TextButton")
			local sliderBtnFrame = Instance.new("Frame")
			
			local Value
			local dragging = false

			sliderFrame.Name = "sliderFrame"
			sliderFrame.Parent = tabFrame
			sliderFrame.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
			sliderFrame.BorderSizePixel = 0
			sliderFrame.Position = UDim2.new(0.0442176871, 0, 0.661691546, 0)
			sliderFrame.Size = UDim2.new(0, 134, 0, 50)

			sliderText.Name = "sliderText"
			sliderText.Parent = sliderFrame
			sliderText.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			sliderText.BackgroundTransparency = 1.000
			sliderText.BorderSizePixel = 0
			sliderText.Position = UDim2.new(0, 0, 0.0666666701, 0)
			sliderText.Size = UDim2.new(0, 80, 0, 19)
			sliderText.Font = Enum.Font.Gotham
			sliderText.Text = "  " .. text
			sliderText.TextColor3 = Color3.fromRGB(255, 255, 255)
			sliderText.TextSize = 14.000
			sliderText.TextWrapped = true
			sliderText.TextXAlignment = Enum.TextXAlignment.Left

			sliderValue.Name = "sliderValue"
			sliderValue.Parent = sliderFrame
			sliderValue.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			sliderValue.BackgroundTransparency = 1.000
			sliderValue.BorderSizePixel = 0
			sliderValue.Position = UDim2.new(0.597014904, 0, 0.0666666701, 0)
			sliderValue.Size = UDim2.new(0, 46, 0, 19)
			sliderValue.Font = Enum.Font.Gotham
			sliderValue.Text = default .. "  "
			sliderValue.TextColor3 = Color3.fromRGB(255, 255, 255)
			sliderValue.TextSize = 14.000
			sliderValue.TextWrapped = true
			sliderValue.TextXAlignment = Enum.TextXAlignment.Right

			sliderButton.Name = "sliderButton"
			sliderButton.Parent = sliderFrame
			sliderButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
			sliderButton.BorderSizePixel = 0
			sliderButton.Position = UDim2.new(0.0597014911, 0, 0.692592621, 0)
			sliderButton.Size = UDim2.new(0, 118, 0, 4)
			sliderButton.AutoButtonColor = false
			sliderButton.Font = Enum.Font.SourceSans
			sliderButton.Text = ""
			sliderButton.TextColor3 = Color3.fromRGB(0, 0, 0)
			sliderButton.TextSize = 14.000

			sliderBtnFrame.Parent = sliderButton
			sliderBtnFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			sliderBtnFrame.BorderSizePixel = 0
			sliderBtnFrame.Size = UDim2.new((default - min) / (max - min), 0, 0, 4) -- UDim2.new(0, 0, 0, 4)
			
			Resize(53)
			
			local function move(input)
				local pos = 
					UDim2.new(
						math.clamp((input.Position.X - sliderButton.AbsolutePosition.X) / sliderButton.AbsoluteSize.X, 0, 1),
						0,
						0,
						6
					)
				sliderBtnFrame:TweenSize(pos, "Out", "Sine", 0.1, true)
				Value = (((pos.X.Scale * max) / max) * (max - min) + min)
				Value = round(Value, float)
				Value = math.clamp(Value, min, max)
				sliderValue.Text = Value
				callback(Value)
			end

			local function moveEnded(input)
				local pos = 
					UDim2.new(
						math.clamp((input.Position.X - sliderButton.AbsolutePosition.X) / sliderButton.AbsoluteSize.X, 0, 1),
						0,
						0,
						4
					)
				sliderBtnFrame:TweenSize(pos, "Out", "Sine", 0.1, true)
				Value = (((pos.X.Scale * max) / max) * (max - min) + min)
				Value = round(Value, float)
				Value = math.clamp(Value, min, max)
				sliderValue.Text = Value
				callback(Value)
			end

			sliderButton.InputBegan:Connect(function(input)
				if input.UserInputType == Enum.UserInputType.MouseButton1 then
					dragging = true
				end
			end)

			sliderButton.InputEnded:Connect(function(input)
				if input.UserInputType == Enum.UserInputType.MouseButton1 then
					dragging = false
					moveEnded(input)
				end
			end)

			uis.InputChanged:Connect(function(input)
				if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
					move(input)
				end
			end)
			
			function sliderFunc:Change(tochange)
				sliderBtnFrame.Size = UDim2.new((tochange or 0) / max, 0, 0, 6)
				sliderValue.Text = tostring(tochange and (tochange / max) * (max - min) + min) or 0
				callback(tochange)
			end
			
			return sliderFunc
		end
		
		function elements:CreateBox(text, preset, callback)
			text = text or "Box"
			preset = preset or "Text"
			callback = callback or function() end
			
			callback(preset)
			
			local tbFrame = Instance.new("Frame")
			local tbText = Instance.new("TextLabel")
			local tbMain = Instance.new("TextBox")

			tbFrame.Name = "tbFrame"
			tbFrame.Parent = tabFrame
			tbFrame.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
			tbFrame.BorderSizePixel = 0
			tbFrame.Position = UDim2.new(0.0442176871, 0, 0.661691546, 0)
			tbFrame.Size = UDim2.new(0, 134, 0, 30)

			tbText.Name = "tbText"
			tbText.Parent = tbFrame
			tbText.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			tbText.BackgroundTransparency = 1.000
			tbText.BorderSizePixel = 0
			tbText.Size = UDim2.new(0, 80, 0, 30)
			tbText.Font = Enum.Font.Gotham
			tbText.Text = "  " .. text
			tbText.TextColor3 = Color3.fromRGB(255, 255, 255)
			tbText.TextSize = 14.000
			tbText.TextWrapped = true
			tbText.TextXAlignment = Enum.TextXAlignment.Left

			tbMain.Name = "tbMain"
			tbMain.Parent = tbFrame
			tbMain.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
			tbMain.BorderSizePixel = 0
			tbMain.Position = UDim2.new(0.626865625, 0, 0.187036127, 0)
			tbMain.Size = UDim2.new(0, 42, 0, 17)
			tbMain.Font = Enum.Font.Gotham
			tbMain.PlaceholderText = "Text"
			tbMain.Text = preset
			tbMain.TextColor3 = Color3.fromRGB(255, 255, 255)
			tbMain.TextSize = 13.000
			tbMain.TextWrapped = true
			tbMain.FocusLost:Connect(function()
				callback(tbMain.Text)
			end)
			
			Resize(30)
		end
		
		function elements:CreateDropdown(text, list, default, callback)
			text = text or "Dropdown"
			list = list or {}
			default = default or ""
			callback = callback or function() end
			
			local dropVis = false
			
			callback(default)
			
			local dpFrame = Instance.new("Frame")
			local dpText = Instance.new("TextLabel")
			local dpBtn = Instance.new("TextButton")
			local dpListContainer = Instance.new("Frame")
			local dpListlayout = Instance.new("UIListLayout")

			dpFrame.Name = "dpFrame"
			dpFrame.Parent = tabFrame
			dpFrame.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
			dpFrame.BorderSizePixel = 0
			dpFrame.Position = UDim2.new(0.0442176871, 0, 0.661691546, 0)
			dpFrame.Size = UDim2.new(0, 134, 0, 30)

			dpText.Name = "dpText"
			dpText.Parent = dpFrame
			dpText.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			dpText.BackgroundTransparency = 1.000
			dpText.BorderSizePixel = 0
			dpText.Size = UDim2.new(0, 80, 0, 30)
			dpText.Font = Enum.Font.Gotham
			dpText.Text = "  " .. text
			dpText.TextColor3 = Color3.fromRGB(255, 255, 255)
			dpText.TextSize = 14.000
			dpText.TextWrapped = true
			dpText.TextXAlignment = Enum.TextXAlignment.Left

			dpBtn.Name = "dpBtn"
			dpBtn.Parent = dpFrame
			dpBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			dpBtn.BackgroundTransparency = 1.000
			dpBtn.BorderSizePixel = 0
			dpBtn.Position = UDim2.new(0.810000062, 0, 0.220369473, 0)
			dpBtn.Size = UDim2.new(0, 17, 0, 15)
			dpBtn.AutoButtonColor = false
			dpBtn.Font = Enum.Font.Gotham
			dpBtn.Text = "+"
			dpBtn.TextColor3 = Color3.fromRGB(165, 165, 165)
			dpBtn.TextScaled = true
			dpBtn.TextSize = 14.000
			dpBtn.TextWrapped = true
			dpBtn.MouseButton1Click:Connect(function()
				dropVis = not dropVis
				if dropVis then
					TweenService:Create(dpBtn, TweenInfo.new(0.3, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
						Rotation = 359,
						TextColor3 = Color3.fromRGB(255, 255, 255),
						TextSize = 21
					}):Play()
					dpListContainer.Visible = true
				else
					TweenService:Create(dpBtn, TweenInfo.new(0.3, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
						Rotation = 0,
						TextColor3 = Color3.fromRGB(165, 165, 165),
						TextSize = 14
					}):Play()
					dpListContainer.Visible = false
				end
			end)
			
			for i,v in next, list do
				local dpListBtn = Instance.new("TextButton")
				
				dpListBtn.Name = "dpListBtn"
				dpListBtn.Parent = dpListContainer
				dpListBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
				dpListBtn.BorderSizePixel = 0
				dpListBtn.Size = UDim2.new(0, 134, 0, 30)
				dpListBtn.AutoButtonColor = false
				dpListBtn.Font = Enum.Font.Gotham
				dpListBtn.Text = "  " .. v
				dpListBtn.TextColor3 = Color3.fromRGB(165, 165, 165)
				dpListBtn.TextSize = 14.000
				dpListBtn.TextXAlignment = Enum.TextXAlignment.Left
				dpListBtn.MouseButton1Click:Connect(function()
					for _,b in pairs(dpListContainer:GetChildren()) do
						if b:IsA("TextButton") then
							TweenService:Create(b, TweenInfo.new(0.12, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
								TextColor3 = Color3.fromRGB(165, 165, 165)
							}):Play()
						end
					end
					TweenService:Create(dpListBtn, TweenInfo.new(0.12, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
						TextColor3 = Color3.fromRGB(255, 255, 255)
					}):Play()
					callback(v)
				end)
				
				if dpListBtn.Text == "  " .. default then
					dpListBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
				end
			end

			dpListContainer.Name = "dpListContainer"
			dpListContainer.Parent = dpFrame
			dpListContainer.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			dpListContainer.BackgroundTransparency = 1.000
			dpListContainer.BorderSizePixel = 0
			dpListContainer.Position = UDim2.new(1.13432837, 0, 0, 0)
			dpListContainer.Size = UDim2.new(0, 139, 0, 367)
			dpListContainer.Visible = false

			dpListlayout.Name = "dpListlayout"
			dpListlayout.Parent = dpListContainer
			dpListlayout.SortOrder = Enum.SortOrder.LayoutOrder
			
			Resize(33)
		end

		function elements:CreateLabel(text, t)
			local Text = Instance.new("TextLabel")

			Text.Name = "tbText"
			Text.Parent = tabFrame
			Text.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			Text.BackgroundTransparency = 1.000
			Text.BorderSizePixel = 0
			Text.Size = UDim2.new(0, 134, 0, 30)
			Text.Font = Enum.Font.Gotham
			Text.Text = "  " .. text
			Text.TextColor3 = Color3.fromRGB(255, 255, 255)
			Text.TextSize = 14.000
			Text.TextScaled = t
			Text.TextXAlignment = Enum.TextXAlignment.Left

			Resize(30)
		end
		
		return elements
	end
	
	return tab
end

return lib
