--[=[
 d888b  db    db d888888b      .d888b.      db      db    db  .d8b.  
88' Y8b 88    88   `88'        VP  `8D      88      88    88 d8' `8b 
88      88    88    88            odD'      88      88    88 88ooo88 
88  ooo 88    88    88          .88'        88      88    88 88~~~88 
88. ~8~ 88b  d88   .88.        j88.         88booo. 88b  d88 88   88 
 Y888P  ~Y8888P' Y888888P      888888D      Y88888P ~Y8888P' YP   YP  CONVERTER
]=]

-- Instances: 7 | Scripts: 1 | Modules: 0
local G2L = {};

-- StarterGui.Notifications
G2L["1"] = Instance.new("ScreenGui", game:GetService("CoreGui"));
G2L["1"]["Name"] = [[Notifications]];
G2L["1"]["ZIndexBehavior"] = Enum.ZIndexBehavior.Sibling;
G2L["1"]["ResetOnSpawn"] = false;
G2L["1"]["DisplayOrder"] = 999999;

-- StarterGui.Notifications.LocalScript
G2L["2"] = Instance.new("LocalScript", G2L["1"]);


-- StarterGui.Notifications.LocalScript.Template
G2L["3"] = Instance.new("Frame", G2L["2"]);
G2L["3"]["BorderSizePixel"] = 0;
G2L["3"]["BackgroundColor3"] = Color3.fromRGB(0, 0, 0);
G2L["3"]["AnchorPoint"] = Vector2.new(0.5, 0.5);
G2L["3"]["BackgroundTransparency"] = 0.5;
G2L["3"]["Size"] = UDim2.new(0.13699999451637268, 0, 0.025, 0);
G2L["3"]["BorderColor3"] = Color3.fromRGB(0, 0, 0);
G2L["3"]["Position"] = UDim2.new(0.5, 0, 0.8847006559371948, 0);
G2L["3"]["Name"] = [[Template]];

-- StarterGui.Notifications.LocalScript.Template.UICorner
G2L["4"] = Instance.new("UICorner", G2L["3"]);
G2L["4"]["CornerRadius"] = UDim.new(1, 0);

-- StarterGui.Notifications.LocalScript.Template.Label
G2L["5"] = Instance.new("TextLabel", G2L["3"]);
G2L["5"]["BorderSizePixel"] = 0;
G2L["5"]["RichText"] = true;
G2L["5"]["BackgroundColor3"] = Color3.fromRGB(255, 255, 255);
G2L["5"]["FontFace"] = Font.new([[rbxasset://fonts/families/Ubuntu.json]], Enum.FontWeight.Regular, Enum.FontStyle.Normal);
G2L["5"]["TextSize"] = 14;
G2L["5"]["TextColor3"] = Color3.fromRGB(255, 255, 255);
G2L["5"]["Size"] = UDim2.new(1, 0, 1, 0);
G2L["5"]["BorderColor3"] = Color3.fromRGB(0, 0, 0);
G2L["5"]["Text"] = [[example]];
G2L["5"]["Name"] = [[Label]];
G2L["5"]["BackgroundTransparency"] = 1;

-- StarterGui.Notifications.LocalScript.Template.Label.UIPadding
G2L["6"] = Instance.new("UIPadding", G2L["5"]);
G2L["6"]["PaddingTop"] = UDim.new(0.20000000298023224, 0);
G2L["6"]["PaddingRight"] = UDim.new(0.20000000298023224, 0);
G2L["6"]["PaddingBottom"] = UDim.new(0.20000000298023224, 0);
G2L["6"]["PaddingLeft"] = UDim.new(0.20000000298023224, 0);

-- StarterGui.Notifications.LocalScript.Template.Label.UITextSizeConstraint
G2L["7"] = Instance.new("UITextSizeConstraint", G2L["5"]);
G2L["7"]["MaxTextSize"] = 18;

-- StarterGui.Notifications.LocalScript
local function C_2()
local script = G2L["2"];
	local TweenService = game:GetService("TweenService")
	local TextService = game:GetService("TextService")
	local Active = 0
	local Notifications = {}
	
	local Library = {}
	
	function Library:Notify(Text: string, Time: number)
		local PrePosition = UDim2.new(0.5, 0, 1.1, 0)
		local PostPosition = UDim2.new(0.5, 0, 0.885 - (Active / 15), 0)
		Active += 1
	
		local Template = script.Template:Clone()
		Template.Parent = script.Parent
		Template.Label.Text = Text
		Template.Position = PrePosition
	
		local textSize = TextService:GetTextSize(Text, Template.Label.TextSize, Template.Label.Font, Vector2.new(10000, Template.Label.AbsoluteSize.Y))
		local parentWidth = Template.Parent.AbsoluteSize.X
		local newScaleX = ((textSize.X + 20) / parentWidth)
	
		Template.Size = UDim2.new(newScaleX, 0, Template.Size.Y.Scale, 0)
	
		table.insert(Notifications, Template)
	
		local Post = TweenService:Create(Template, TweenInfo.new(1, Enum.EasingStyle.Quint, Enum.EasingDirection.Out), {Position = PostPosition})
		local Pre = TweenService:Create(Template, TweenInfo.new(1, Enum.EasingStyle.Quint, Enum.EasingDirection.Out), {Position = PrePosition})
	
		task.spawn(function()
			Post:Play()
			Post.Completed:Wait()
			task.wait(Time)
			Pre:Play()
			for i, v in ipairs(Notifications) do
				if v == Template then
					table.remove(Notifications, i)
					break
				end
			end
			Active -= 1
			self:UpdatePositions()
			Pre.Completed:Wait()
			Template:Destroy()
		end)
	end
	
	function Library:UpdatePositions()
		for i, notification in ipairs(Notifications) do
			local NewPosition = UDim2.new(0.5, 0, 0.885 - ((i - 1) / 15), 0)
			local Tween = TweenService:Create(notification, TweenInfo.new(1, Enum.EasingStyle.Quint, Enum.EasingDirection.Out), {Position = NewPosition})
			Tween:Play()
		end
	end
  
	return Library;
end;
local Library = C_2();

return Library, G2L["1"];
