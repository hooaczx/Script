local lib = {}
local table1 = {}
local _SGUI = Instance.new('ScreenGui',game:GetService('CoreGui'))
_SGUI.Name = 'FE2Notifs'
local TopFrame = Instance.new("Frame")
local TopText = Instance.new("TextLabel")
local UIAspectRatioConstraint = Instance.new("UIAspectRatioConstraint")

TopFrame.Name = ";3"
TopFrame.Parent = game:GetService('CoreGui').FE2Notifs
TopFrame.BackgroundColor3 = Color3.new(1, 1, 1)
TopFrame.BackgroundTransparency = 1
TopFrame.Position = UDim2.new(0.371764719, 0, 0, 0)
TopFrame.Size = UDim2.new(0.255882353, 0, 0.0865671635, 0)

UIAspectRatioConstraint.Parent = TopFrame
UIAspectRatioConstraint.AspectRatio = 8.039999961853027

function lib.handleAlert(p56, p57, p58, p59)
	if p57.Parent == game:GetService('CoreGui').FE2Notifs[';3'] then
		local v48 = (p56 > 4 or p58) and p56 or p56 - 1;
		local v49 = (p56 > 4 or p58) and "In" or "Out";
		if p56 > 4 or p58 then
			p57.ZIndex = 0;
			table.remove(table1, p56);
			local TweenInfo_new_ret13 = TweenInfo.new(0.3, Enum.EasingStyle.Quint, Enum.EasingDirection.In);
			game:GetService("TweenService"):Create(p57.Alert_Shadow, TweenInfo_new_ret13, {
				ImageTransparency = 1
			}):Play();
			game:GetService("TweenService"):Create(p57.Alert, TweenInfo_new_ret13, {
				TextTransparency = 1.1,
				TextStrokeTransparency = 1
			}):Play();
			task.delay(0.3, function()
				p57:Destroy();
			end);
			return;
		end
		p57:TweenPosition(UDim2.new(0.5, 0, v48, 0), v49, "Sine", 0.325, true);
	end
end

function lib.alert(text, clr, tim, special)
	task.spawn(function()
		local _TIME = tim or 4.5;
		local Frame = Instance.new("Frame");
		local ImageLabel = Instance.new("ImageLabel");
		local TextLabel = Instance.new("TextLabel");
		Frame.Name = "Alert_Frame";
		Frame.Size = UDim2.new(1, 0, 1, 0);
		Frame.Position = UDim2.new(0.5, 0, -1, 0);
		Frame.AnchorPoint = Vector2.new(0.5, 0);
		Frame.BackgroundTransparency = 1;
		local UIScale = Instance.new("UIScale");
		UIScale.Scale = 0;
		UIScale.Parent = Frame;
		ImageLabel.Name = "Alert_Shadow";
		ImageLabel.BackgroundTransparency = 1;
		ImageLabel.Image = "rbxassetid://1057492965";
		ImageLabel.ImageTransparency = 0.5;
		ImageLabel.Size = UDim2.new(1, 0, 1, 0);
		ImageLabel.Parent = Frame;
		TextLabel.Name = "Alert";
		TextLabel.ZIndex = 3;
		TextLabel.Text = text;
		TextLabel.TextColor3 = clr or Color3.new(1, 1, 1);
		TextLabel.TextScaled = true;
		TextLabel.Font = "Ubuntu";
		TextLabel.TextStrokeTransparency = 0;
		TextLabel.BackgroundTransparency = 1;
		TextLabel.BackgroundColor3 = Color3.new();
		TextLabel.BorderSizePixel = 0;
		TextLabel.Size = UDim2.new(1, 0, 1, 0);
		local UIGradient = Instance.new("UIGradient");
		UIGradient.Color = ColorSequence.new(Color3.fromRGB(255, 255, 255), Color3.fromRGB(159, 159, 159));
		UIGradient.Rotation = 90;
		UIGradient.Parent = TextLabel;
		TextLabel.Parent = Frame;
		Frame.Parent = game:GetService('CoreGui').FE2Notifs[';3'];
		if _TIME > 8 then
			local Frame2 = Instance.new("Frame", TextLabel);
			Frame2.BorderSizePixel = 0;
			Frame2.BackgroundColor3 = clr or Color3.new(1, 1, 1);
			Frame2.AnchorPoint = Vector2.new(0.5, 0.5);
			Frame2.Position = UDim2.new(0.5);
			Frame2.Size = UDim2.new(1, 0, 0.1, 0);
			Frame2:TweenSize(UDim2.new(0, 0, 0.1, 0), "Out", "Linear", _TIME);
			local ImageLabel2 = Instance.new("ImageLabel", Frame2);
			ImageLabel2.Image = "rbxassetid://156579757";
			ImageLabel2.Size = UDim2.new(1, 0, 1, 0);
			ImageLabel2.BackgroundTransparency = 1;
			ImageLabel2.ImageTransparency = 0.5;
		end
		if special == "rainbow" then
			task.spawn(function()
				local v85 = 0;
				while TextLabel and task.wait() do
					if v85 > 1 then
						v85 = 0;
					end
					TextLabel.TextColor3 = Color3.fromHSV(v85, 1, 1);
					v85 = v85 + 1 / 120;
				end
			end);
		end
		local v55 = 1;
		table.insert(table1, v55, Frame);
		for index6 = 1, #table1 do
			lib.handleAlert(index6, table1[index6]);
		end
		local TweenInfo_new_ret14 = TweenInfo.new(0.325, Enum.EasingStyle.Quint, Enum.EasingDirection.Out);
		game:GetService("TweenService"):Create(Frame.UIScale, TweenInfo_new_ret14, {
			Scale = 1
		}):Play();
		game:GetService("TweenService"):Create(TextLabel, TweenInfo_new_ret14, {
			TextTransparency = 0,
			TextStrokeTransparency = 0.1
		}):Play();
		task.wait(_TIME)
		if Frame and TextLabel and TextLabel.TextTransparency <= 0 then
			local v78 = 1;
			for index10 = 1, #table1 do
				if table1[index10] == Frame then
					v78 = index10;
					break;
				end
			end
			lib.handleAlert(v78, Frame, true, _TIME);
		end
		for index7 = 1, #table1 do
			lib.handleAlert(index7, table1[index7]);
		end
		return TextLabel;
	end)
end

return lib
