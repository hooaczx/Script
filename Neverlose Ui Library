-- Services
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local HttpService = game:GetService("HttpService")
local TweenService = game:GetService("TweenService")
local ContentProvider = game:GetService("ContentProvider")
local MarketplaceService = game:GetService("MarketplaceService")
local LocalizationService = game:GetService("LocalizationService")

-- Functions
local function FormatTime(Time)
    local Years = math.floor(Time / 31104000)
    local RemainingTime = Time % 31104000
    local Months = math.floor(RemainingTime / 2592000)
    RemainingTime = RemainingTime % 2592000
    local Weeks = math.floor(RemainingTime / 604800)
    RemainingTime = RemainingTime % 604800
    local Days = math.floor(RemainingTime / 86400)
    RemainingTime = RemainingTime % 86400
    local Hours = math.floor(RemainingTime / 3600)
    RemainingTime = RemainingTime % 3600
    local Minutes = math.floor(RemainingTime / 60)
    RemainingTime = RemainingTime % 60
    local Seconds = RemainingTime

    if Years ~= 0 then
        return ("%s year%s ago"):format(Years, (Years > 1 and "s" or ""))
    elseif Months ~= 0 then
        return ("%s month%s ago"):format(Months, (Months > 1 and "s" or ""))
    elseif Weeks ~= 0 then
        return ("%s week%s ago"):format(Weeks, (Weeks > 1 and "s" or ""))
    elseif Days ~= 0 then
        return ("%s day%s ago"):format(Days, (Days > 1 and "s" or ""))
    elseif Hours ~= 0 then
        return ("%s hour%s ago"):format(Hours, (Hours > 1 and "s" or ""))
    elseif Minutes ~= 0 then
        return ("%s minute%s ago"):format(Minutes, (Minutes > 1 and "s" or ""))
    elseif Seconds ~= 0 then
        return ("%s second%s ago"):format(Seconds, (Seconds > 1 and "s" or ""))
    end
end

local function TweenObject(Object, Duration, Properties)
    TweenService:Create(Object, TweenInfo.new(Duration, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut), Properties):Play()
end

local function Names(Text)
	local Names = {}
	
	for Name in Text:gmatch("[^,%s]+") do
		if Name ~= "Creator" and Name:lower() ~= "and" then
			table.insert(Names, Name)
		end
	end

	return #Names
end

local function CheckType(Object, Type, _Default)
	if typeof(Object) == Type then
		return Object
	end

	return typeof(_Default) == Type and _Default or nil
end

local function Translate(Text, State)
	Text = CheckType(Text, "string", nil)

	if Text then
		State = CheckType(State, "boolean", true)

		local StartIndx, EndIndx, Captured = Text:find("{(.-)}")
		local Source, Target = "auto", LocalizationService.RobloxLocaleId:match("(%w+)-")

		local Url = ("https://translate.googleapis.com/translate_a/single?client=gtx&sl=%s&tl=%s&dt=t&q=%s"):format(Source, Target, HttpService:UrlEncode(Captured or Text))
		local Result = HttpService:JSONDecode(game:HttpGet(Url))

		if Result[3] ~= Target and State then
			if Captured then
				local NewText = Text:gsub("{(.-)}", Result[1][1][1])
				return NewText
			else
				return Result[1][1][1]
			end
		else
			Text = Text:gsub("[{}]", "")
			return Text
		end
	end
end

-- Library
local Library = {}

pcall(function()
    OldInstance:Destroy()
end)

local NewInstance = Instance.new("ScreenGui")
NewInstance.Name = "Loady"

if gethui then
	NewInstance.Parent = gethui()
elseif not is_sirhurt_closure and (syn and syn.protect_gui) then
	syn.protect_gui(NewInstance)
	NewInstance.Parent = CoreGui
else
	NewInstance.Parent = CoreGui
end

getgenv().OldInstance = NewInstance

function Library:Window(Title, Func, Options)
    Title = CheckType(Title, "string", "Loady")
    Func = CheckType(Func, "function", function() end)

	Options = CheckType(Options, "table", {})

	Options.Gradient = CheckType(Options.Gradient, "table", {})
    Options.Gradient.Start = CheckType(Options.Gradient.Start, "Color3", Color3.fromRGB(1, 180, 213))
    Options.Gradient.End = CheckType(Options.Gradient.End, "Color3", Color3.fromRGB(3, 227, 122))
	Options.Gradient.Rotation = CheckType(Options.Gradient.Rotation, "number", 0)

	Options.Translate = CheckType(Options.Translate, "boolean", true)

    local WindowData = {
        Created = 0,
        Selection = nil
    }

    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "Main"
    MainFrame.ClipsDescendants = true
	MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
	MainFrame.BackgroundColor3 = Color3.fromRGB(7, 7, 7)
	MainFrame.BorderColor3 = Color3.fromRGB(53, 53, 53)
	MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
	MainFrame.Size = UDim2.new(0, 0, 0, 0)
    MainFrame.Parent = NewInstance

    local ShadeFrame = Instance.new("Frame")
    ShadeFrame.Name = "Shade"
	ShadeFrame.AnchorPoint = Vector2.new(0.5, 0.5)
	ShadeFrame.BackgroundColor3 = Color3.fromRGB(7, 7, 7)
	ShadeFrame.BorderColor3 = Color3.fromRGB(53, 53, 53)
	ShadeFrame.BorderSizePixel = 0
    ShadeFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
    ShadeFrame.Size = UDim2.new(0, 624, 0, 430)
    ShadeFrame.ZIndex = 10
    ShadeFrame.Parent = MainFrame

    local Loader = Instance.new("Frame")
    Loader.Name = "Loader"
	Loader.BackgroundColor3 = Color3.fromRGB(26, 26, 26)
	Loader.Position = UDim2.new(0.322, 0, 0.26315799355506897, 0)
	Loader.Size = UDim2.new(0, 388, 0, 35)
    Loader.Parent = MainFrame

    local LoaderCorner = Instance.new("UICorner")
    LoaderCorner.Name = "Corner"
    LoaderCorner.CornerRadius = UDim.new(0, 6)
    LoaderCorner.Parent = Loader

    local Splash = Instance.new("Frame")
    Splash.Name = "Splash"
	Splash.BorderSizePixel = 0
	Splash.BackgroundColor3 = Color3.fromRGB(170, 170, 170)
	Splash.Size = UDim2.new(0, 0, 0, 35)
	Splash.ZIndex = 2
	Splash.Parent = Loader

    local SplashCorner = Instance.new("UICorner")
    SplashCorner.Name = "Corner"
    SplashCorner.CornerRadius = UDim.new(0, 6)
    SplashCorner.Parent = Splash

    local SplashGradient = Instance.new("UIGradient")
    SplashGradient.Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0.00, Options.Gradient.Start),
		ColorSequenceKeypoint.new(1.00, Options.Gradient.End)
	})
	SplashGradient.Rotation = Options.Gradient.Rotation
    SplashGradient.Parent = Splash

	local WelcomeLabel = Instance.new("TextLabel")
	WelcomeLabel.Name = "WelcomeLabel"
	WelcomeLabel.BackgroundColor3 = Color3.fromRGB(26, 26, 26)
	WelcomeLabel.BackgroundTransparency = 1
	WelcomeLabel.BorderColor3 = Color3.fromRGB(26, 26, 26)
	WelcomeLabel.BorderSizePixel = 0
	WelcomeLabel.Size = UDim2.new(0, 388, 0, 35)
	WelcomeLabel.Font = Enum.Font.Gotham
	WelcomeLabel.RichText = true
	WelcomeLabel.Text = Translate(("{Hi}, %s!"):format(Players.LocalPlayer.DisplayName), Options.Translate)
	WelcomeLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
	WelcomeLabel.TextSize = 14.000
	WelcomeLabel.Parent = Loader

    local MainCorner = Instance.new("UICorner")
    MainCorner.Name = "Corner"
    MainCorner.CornerRadius = UDim.new(0, 6)
    MainCorner.Parent = MainFrame

    local Cloud = Instance.new("ImageButton")
    Cloud.Name = "Cloud"
	Cloud.AnchorPoint = Vector2.new(0.5, 0.5)
	Cloud.BackgroundTransparency = 1
	Cloud.Position = UDim2.new(0.093952976167202, 0, 0.09986995160579681, 0)
	Cloud.Size = UDim2.new(0, 35, 0, 35)
	Cloud.ZIndex = 2
	Cloud.Image = "rbxassetid://3926305904"
	Cloud.ImageRectOffset = Vector2.new(324, 764)
	Cloud.ImageRectSize = Vector2.new(36, 36)
    Cloud.Parent = MainFrame

    local CloudGradient = Instance.new("UIGradient")
    CloudGradient.Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0.00, Options.Gradient.Start),
		ColorSequenceKeypoint.new(1.00, Options.Gradient.End)
	})
	CloudGradient.Rotation = Options.Gradient.Rotation
    CloudGradient.Parent = Cloud

    local MainTitle = Instance.new("TextLabel")
    MainTitle.Name = "Title"
	MainTitle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	MainTitle.BackgroundTransparency = 1
	MainTitle.Position = UDim2.new(0.1465577334165573, 0, 0.062476783990859985, 0)
	MainTitle.Size = UDim2.new(0, 193, 0, 35)
	MainTitle.Font = Enum.Font.Gotham
	MainTitle.RichText = true
	MainTitle.Text = ("<b>%s</b>"):format(Title)
	MainTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
	MainTitle.TextSize = 15
	MainTitle.TextXAlignment = Enum.TextXAlignment.Left
    MainTitle.Parent = MainFrame

    local ContentFrame = Instance.new("Frame")
    ContentFrame.Name = "Content"
	ContentFrame.Visible = false
	ContentFrame.AnchorPoint = Vector2.new(0.5, 0)
	ContentFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	ContentFrame.BackgroundTransparency = 1
	ContentFrame.Position = UDim2.new(0.5, 0, 0.165, 0)
	ContentFrame.Size = UDim2.new(0, 547, 0, 345)
	ContentFrame.Transparency = 1
    ContentFrame.Parent = MainFrame

    local Search = Instance.new("TextBox")
    Search.Name = "Search"
    Search.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    Search.BorderColor3 = Color3.fromRGB(20, 20, 20)
    Search.BorderSizePixel = 0
    Search.LayoutOrder = -5
    Search.Position = UDim2.new(0.00914076809, 0, 0.139130428, 0)
    Search.Size = UDim2.new(0.481718451, 0, -0.220234364, 50)
    Search.Font = Enum.Font.Gotham
    Search.PlaceholderText = Translate("Search...", Options.Translate)
    Search.Text = ""
    Search.TextColor3 = Color3.fromRGB(255, 255, 255)
    Search.TextSize = 14.000
    Search.TextStrokeColor3 = Color3.fromRGB(255, 255, 255)
    Search.TextWrapped = true
    Search.Parent = ContentFrame

    local SearchCorner = Instance.new("UICorner")
    SearchCorner.CornerRadius = UDim.new(0, 999)
    SearchCorner.Name = "SearchCorner"
    SearchCorner.Parent = Search

    local SelectGame = Instance.new("ScrollingFrame")
    SelectGame.Name = "SelectGame"
	SelectGame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	SelectGame.BackgroundTransparency = 1
	SelectGame.Position = UDim2.new(0, 0, 0.156521872, 0)
	SelectGame.Size = UDim2.new(0.5, 0, -0.156521827, 345)
	SelectGame.ScrollBarThickness = 3
	SelectGame.ScrollBarImageTransparency = 0.2
	SelectGame.Transparency = 1
	SelectGame.BorderSizePixel = 0
	SelectGame.CanvasSize = UDim2.fromScale(0, 0)
    SelectGame.Parent = ContentFrame

    local GameLayout = Instance.new("UIListLayout")
    GameLayout.SortOrder = Enum.SortOrder.LayoutOrder
	GameLayout.Padding = UDim.new(0, 7)
    GameLayout.Parent = SelectGame

	GameLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
		SelectGame.CanvasSize = UDim2.fromOffset(0, GameLayout.AbsoluteContentSize.Y + 5)
	end)

    local GamePadding = Instance.new("UIPadding")
    GamePadding.PaddingLeft = UDim.new(0, 5)
	GamePadding.PaddingRight = UDim.new(0, 10)
    GamePadding.Parent = SelectGame

    local Details = Instance.new("Frame")
    Details.Name = "Details"
	Details.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Details.BackgroundTransparency = 1
	Details.Position = UDim2.new(0.5, 0, 0, 0)
	Details.Size = UDim2.new(0.5, 0, 0.104, 345)
	Details.Transparency = 1
    Details.Parent = ContentFrame

    local DetailsLayout = Instance.new("UIListLayout")
    DetailsLayout.Padding = UDim.new(0, 7)
    DetailsLayout.Parent = Details

    local DetailsPadding = Instance.new("UIPadding")
    DetailsPadding.PaddingLeft = UDim.new(0, 5)
	DetailsPadding.PaddingRight = UDim.new(0, 0)
    DetailsPadding.Parent = Details

    local AImage = Instance.new("ImageButton")
    AImage.Name = "AImage"
	AImage.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	AImage.Size = UDim2.new(1, 0, 0.08, 100)
	AImage.Image = "http://www.roblox.com/asset/?id=8899558070"
    AImage.Parent = Details

    local ImageCorner = Instance.new("UICorner")
    ImageCorner.CornerRadius = UDim.new(0, 7)
    ImageCorner.Parent = AImage

    local ImageFrame = Instance.new("Frame")
    ImageFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	ImageFrame.BackgroundTransparency = 0.35
	ImageFrame.Size = UDim2.new(1, 0, 1, 0)
	ImageFrame.Parent = AImage

    local IFrameCorner = Instance.new("UICorner")
    IFrameCorner.CornerRadius = UDim.new(0, 7)
    IFrameCorner.Parent = ImageFrame

    local IFrameGradient = Instance.new("UIGradient")
    IFrameGradient.Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0.00, Options.Gradient.Start),
		ColorSequenceKeypoint.new(1.00, Options.Gradient.End)
	})
	IFrameGradient.Rotation = Options.Gradient.Rotation
    IFrameGradient.Parent = ImageFrame

    local ImageLabel = Instance.new("TextLabel")
    ImageLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	ImageLabel.BackgroundTransparency = 1
	ImageLabel.Position = UDim2.new(0.0759013295173645, 0, 0.7128929495811462, 0)
	ImageLabel.Size = UDim2.new(0, 200, 0, 29)
	ImageLabel.Font = Enum.Font.Gotham
	ImageLabel.RichText = true
	ImageLabel.Text = ""
	ImageLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
	ImageLabel.TextSize = 14
	ImageLabel.TextXAlignment = Enum.TextXAlignment.Left
    ImageLabel.Parent = AImage

    local YLoad = Instance.new("TextButton")
    YLoad.Name = "YLoad"
	YLoad.BackgroundColor3 = Color3.fromRGB(118, 118, 118)
	YLoad.Position = UDim2.new(0, 0, 0.6724890470504761, 0)
	YLoad.Size = UDim2.new(1, 0, -0.02028985507786274, 50)
	YLoad.AutoButtonColor = false
	YLoad.Font = Enum.Font.Gotham
	YLoad.RichText = true
	YLoad.Text = ""
	YLoad.TextColor3 = Color3.fromRGB(255, 255, 255)
	YLoad.TextSize = 9
    YLoad.Parent = Details

    local YLoadCorner = Instance.new("UICorner")
    YLoadCorner.CornerRadius = UDim.new(0, 4)
    YLoadCorner.Parent = YLoad

    local YLoadPadding = Instance.new("UIPadding")
    YLoadPadding.Parent = YLoad

    local YLoadGradient = Instance.new("UIGradient")
    YLoadGradient.Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0.00, Options.Gradient.Start),
		ColorSequenceKeypoint.new(1.00, Options.Gradient.End)
	})
	YLoadGradient.Rotation = Options.Gradient.Rotation
    YLoadGradient.Parent = YLoad

    local PlayButton = Instance.new("ImageButton")
    PlayButton.Name = "Play"
	PlayButton.AnchorPoint = Vector2.new(0.5, 0.5)
	PlayButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	PlayButton.BackgroundTransparency = 1
	PlayButton.Size = UDim2.new(0, 11, 0, 11)
	PlayButton.Image = "rbxassetid://7671465363"
    PlayButton.Parent = YLoad

    local LoadLabel = Instance.new("TextLabel")
    LoadLabel.Name = "LoadLabel"
	LoadLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	LoadLabel.BackgroundTransparency = 1
	LoadLabel.Position = UDim2.new(0, 0, -0.09302325546741486, 0)
	LoadLabel.Size = UDim2.new(0, 263, 0, 50)
	LoadLabel.Font = Enum.Font.Gotham
	LoadLabel.Text = Translate("LOAD", Options.Translate)
	LoadLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
	LoadLabel.TextSize = 11
    LoadLabel.Parent = YLoad

    PlayButton.Position =  UDim2.new(0.5, -(PlayButton.AbsoluteSize.X + LoadLabel.TextBounds.X / 2) + 1, 0.5, 0)

    local TStatus = Instance.new("TextButton")
    TStatus.Name = "TStatus"
	TStatus.RichText = true
	TStatus.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
	TStatus.Position = UDim2.new(0, 0, 0.40002530813217163, 0)
	TStatus.Size = UDim2.new(1, 0, -0.037681158632040024, 50)
	TStatus.AutoButtonColor = false
	TStatus.Font = Enum.Font.Gotham
	TStatus.TextColor3 = Color3.fromRGB(255, 255, 255)
	TStatus.TextSize = 11
	TStatus.Text = ""
	TStatus.TextWrapped = true
	TStatus.TextXAlignment = Enum.TextXAlignment.Left
    TStatus.Parent = Details

    local TStatusCorner = Instance.new("UICorner")
    TStatusCorner.CornerRadius = UDim.new(0, 4)
    TStatusCorner.Parent = TStatus

    local TStatusPadding = Instance.new("UIPadding")
    TStatusPadding.PaddingLeft = UDim.new(0, 20)
    TStatusPadding.Parent = TStatus

    local TStatusGradient = Instance.new("UIGradient")
    TStatusGradient.Color  = ColorSequence.new({
		ColorSequenceKeypoint.new(0.00, Options.Gradient.Start),
		ColorSequenceKeypoint.new(1.00, Options.Gradient.End)
	})
	TStatusGradient.Rotation = Options.Gradient.Rotation
    TStatusGradient.Enabled = false
    TStatusGradient.Parent = TStatus

    local LastUpdateButton = Instance.new("TextButton")
    LastUpdateButton.Name = "XLastUpdated"
	LastUpdateButton.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
	LastUpdateButton.Position = UDim2.new(-0.10246679186820984, 0, 0.41741660237312317, 0)
	LastUpdateButton.Size = UDim2.new(1, 0, -0.037681158632040024, 50)
	LastUpdateButton.AutoButtonColor = false
	LastUpdateButton.Font = Enum.Font.Gotham
	LastUpdateButton.RichText = true
    LastUpdateButton.TextWrapped = true
	LastUpdateButton.Text = ""
	LastUpdateButton.TextColor3 = Color3.fromRGB(255, 255, 255)
	LastUpdateButton.TextSize = 11
	LastUpdateButton.TextXAlignment = Enum.TextXAlignment.Left
    LastUpdateButton.Parent = Details

    local LastUpdateCorner = Instance.new("UICorner")
    LastUpdateCorner.CornerRadius = UDim.new(0, 4)
    LastUpdateCorner.Parent = LastUpdateButton

    local LastUpdatePadding = Instance.new("UIPadding")
    LastUpdatePadding.PaddingLeft = UDim.new(0, 20)
    LastUpdatePadding.Parent = LastUpdateButton

    local LastUpdateGradient = Instance.new("UIGradient")
    LastUpdateGradient.Color  = ColorSequence.new({
		ColorSequenceKeypoint.new(0.00, Options.Gradient.Start),
		ColorSequenceKeypoint.new(1.00, Options.Gradient.End)
	})
	LastUpdateGradient.Rotation = Options.Gradient.Rotation
    LastUpdateGradient.Enabled = false
    LastUpdateGradient.Parent = LastUpdateButton

    local CreatorButton = Instance.new("TextButton")
    CreatorButton.Name = "XLastUpdated"
	CreatorButton.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
	CreatorButton.Position = UDim2.new(-0.10246679186820984, 0, 0.41741660237312317, 0)
	CreatorButton.Size = UDim2.new(1, 0, -0.037681158632040024, 50)
	CreatorButton.AutoButtonColor = false
	CreatorButton.Font = Enum.Font.Gotham
	CreatorButton.RichText = true
	CreatorButton.TextColor3 = Color3.fromRGB(255, 255, 255)
	CreatorButton.TextSize = 11
	CreatorButton.Text = ""
    CreatorButton.TextWrapped = true
	CreatorButton.TextXAlignment = Enum.TextXAlignment.Left
    CreatorButton.Parent = Details

    local CreatorCorner = Instance.new("UICorner")
    CreatorCorner.CornerRadius = UDim.new(0, 4)
    CreatorCorner.Parent = CreatorButton

    local CreatorPadding = Instance.new("UIPadding")
    CreatorPadding.PaddingLeft = UDim.new(0, 20)
    CreatorPadding.Parent = CreatorButton

    local CreatorGradient = Instance.new("UIGradient")
    CreatorGradient.Color  = ColorSequence.new({
		ColorSequenceKeypoint.new(0.00, Options.Gradient.Start),
		ColorSequenceKeypoint.new(1.00, Options.Gradient.End)
	})
	CreatorGradient.Rotation = Options.Gradient.Rotation
    CreatorGradient.Enabled = false
    CreatorGradient.Parent = CreatorButton

    local ZExit = Instance.new("TextButton")
    ZExit.Name = "ZExit"
	ZExit.BackgroundColor3 = Color3.fromRGB(118, 118, 118)
	ZExit.Position = UDim2.new(0, 0, 0.6724890470504761, 0)
	ZExit.Size = UDim2.new(1, 0, -0.02028985507786274, 50)
	ZExit.AutoButtonColor = false
	ZExit.Font = Enum.Font.Gotham
	ZExit.RichText = true
	ZExit.Text = ""
	ZExit.TextColor3 = Color3.fromRGB(255, 255, 255)
	ZExit.TextSize = 9
	ZExit.Parent = Details

    local ZExitCorner = Instance.new("UICorner")
    ZExitCorner.CornerRadius = UDim.new(0, 4)
    ZExitCorner.Parent = ZExit

    local ZExitPadding = Instance.new("UIPadding")
    ZExitPadding.Parent = ZExit

    local ZExitGradient = Instance.new("UIGradient")
    ZExitGradient.Color  = ColorSequence.new({
		ColorSequenceKeypoint.new(0.00, Color3.fromRGB(213, 83, 83)),
		ColorSequenceKeypoint.new(1.00, Color3.fromRGB(227, 34, 34))
	})
    ZExitGradient.Parent = ZExit

    local ExitLabel = Instance.new("TextLabel")
    ExitLabel.Name = "ExitLabel"
	ExitLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	ExitLabel.BackgroundTransparency = 1
	ExitLabel.Position = UDim2.new(0, 0, -0.09302325546741486, 0)
	ExitLabel.Size = UDim2.new(0, 263, 0, 50)
	ExitLabel.Font = Enum.Font.Gotham
	ExitLabel.Text = Translate("EXIT", Options.Translate)
	ExitLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
	ExitLabel.TextSize = 11
    ExitLabel.Parent = ZExit

    local XButton = Instance.new("ImageButton")
    XButton.Name = "X"
	XButton.AnchorPoint = Vector2.new(0.5, 0.5)
	XButton.BackgroundTransparency = 1
	XButton.LayoutOrder = 5
	XButton.Position = UDim2.new(0.3880000114440918, 0, 0.5, 0)
	XButton.Size = UDim2.new(0, 15, 0, 15)
	XButton.ZIndex = 2
	XButton.Image = "rbxassetid://3926305904"
	XButton.ImageRectOffset = Vector2.new(924, 724)
	XButton.ImageRectSize = Vector2.new(36, 36)
    XButton.Parent = ZExit

    XButton.Position =  UDim2.new(0.5, -(XButton.AbsoluteSize.X + ExitLabel.TextBounds.X / 2) + 1, 0.5, 0)

    local GameLabel = Instance.new("TextLabel")
	GameLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	GameLabel.BackgroundTransparency = 1
	GameLabel.Position = UDim2.new(0.011, 0, -0.009, 0)
	GameLabel.Size = UDim2.new(0, 198, 0, 24)
	GameLabel.Font = Enum.Font.Gotham
	GameLabel.RichText = true
	GameLabel.Text = Translate("Select or Search a game", Options.Translate)
	GameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
	GameLabel.TextSize = 14
	GameLabel.TextXAlignment = Enum.TextXAlignment.Left
    GameLabel.Parent = ContentFrame

    YLoad.MouseButton1Down:Connect(function()
		if WindowData.Selection then
        	Func(WindowData.Selection, WindowData)
		end

        WindowData:Exit()
    end)

    ZExit.MouseButton1Down:Connect(function()
        WindowData:Exit()
    end)

    Search:GetPropertyChangedSignal("Text"):Connect(function()
		for _, v in ipairs(SelectGame:GetDescendants()) do
			if v:IsA("TextButton") then
				if Search.Text ~= "" then
					if v.Text:lower():find(Search.Text:lower()) then
						v.Visible = true
					else
						v.Visible = false
					end
				else
					v.Visible = true
				end
			end
		end
	end)

    function WindowData:Game(Selection_Opt)
        WindowData.Created = WindowData.Created + 1

		Selection_Opt.Place = CheckType(Selection_Opt.Place, "number", nil)
        Selection_Opt.UpdateDate = CheckType(Selection_Opt.UpdateDate, "number", nil)
		Selection_Opt.Status = CheckType(Selection_Opt.Status, "string", "Unknown")
		Selection_Opt.Script = CheckType(Selection_Opt.Script, "string", nil)
        Selection_Opt.Creator = CheckType(Selection_Opt.Creator, "string", nil)

		if not Selection_Opt.Place then
			Selection_Opt.Custom = CheckType(Selection_Opt.Custom, "table", {})
			Selection_Opt.Custom.Name = CheckType(Selection_Opt.Custom.Name, "string", Translate("Unknown", Options.Translate))
			Selection_Opt.Custom.Image = CheckType(Selection_Opt.Custom.Image, "string", "")
		end

		local Success, Data = pcall(function()
			return MarketplaceService:GetProductInfo(Selection_Opt.Place)
		end)

		Data = Selection_Opt.Place and ((Success and Data.AssetTypeId == 9) and Data) or Selection_Opt.Custom

        local SelectionButton = Instance.new("TextButton")
        SelectionButton.Name = "SelectionButton"
		SelectionButton.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
		SelectionButton.Size = UDim2.new(1, 0, -0.017391303554177284, 50)
		SelectionButton.AutoButtonColor = false
		SelectionButton.Font = Enum.Font.Gotham
        SelectionButton.TextWrapped = true
		SelectionButton.Text = Data.Name or Translate("Unknown", Options.Translate)
		SelectionButton.TextColor3 = Color3.fromRGB(255, 255, 255)
		SelectionButton.TextSize = 14
		SelectionButton.TextXAlignment = Enum.TextXAlignment.Left
        SelectionButton.Parent = SelectGame

        local SelectionCorner = Instance.new("UICorner")
        SelectionCorner.CornerRadius = UDim.new(0, 4)
        SelectionCorner.Parent = SelectionButton

        local SelectionPadding = Instance.new("UIPadding")
        SelectionPadding.PaddingLeft = UDim.new(0, 50)
        SelectionPadding.Parent = SelectionButton

        local SelectionGradient = Instance.new("UIGradient")
        SelectionGradient.Color = ColorSequence.new({
			ColorSequenceKeypoint.new(0.00, Options.Gradient.Start),
			ColorSequenceKeypoint.new(1.00, Options.Gradient.End)
		})
		SelectionGradient.Enabled = false
		SelectionGradient.Rotation = Options.Gradient.Rotation
        SelectionGradient.Parent = SelectionButton

        local CheckedCircle = Instance.new("ImageButton")
        CheckedCircle.Name = "CheckedCircle"
	    CheckedCircle.ImageTransparency = 1
	    CheckedCircle.AnchorPoint = Vector2.new(0.5, 0.5)
	    CheckedCircle.BackgroundTransparency = 1
	    CheckedCircle.LayoutOrder = 17
	    CheckedCircle.Position = UDim2.new(-0.10421544313430786, 0, 0.5, 0)
	    CheckedCircle.Size = UDim2.new(0, 25, 0, 25)
	    CheckedCircle.ZIndex = 2
	    CheckedCircle.Image = "rbxassetid://3926309567"
	    CheckedCircle.ImageRectOffset = Vector2.new(784, 420)
	    CheckedCircle.ImageRectSize = Vector2.new(48, 48)
        CheckedCircle.Parent = SelectionButton

        local Circle = Instance.new("ImageButton")
        Circle.Name = "Circle"
		Circle.ImageTransparency = 0
		Circle.AnchorPoint = Vector2.new(0.5, 0.5)
		Circle.BackgroundTransparency = 1
		Circle.LayoutOrder = 16
		Circle.Position = UDim2.new(-0.10421544313430786, 0, 0.5, 0)
		Circle.Size = UDim2.new(0, 25, 0, 25)
		Circle.ZIndex = 2
		Circle.Image = "rbxassetid://3926305904"
		Circle.ImageRectOffset = Vector2.new(324, 964)
		Circle.ImageRectSize = Vector2.new(36, 36)
        Circle.Parent = SelectionButton

        local CircleGradient = Instance.new("UIGradient")
        CircleGradient.Color = ColorSequence.new({
			ColorSequenceKeypoint.new(0.00, Options.Gradient.Start),
			ColorSequenceKeypoint.new(1.00, Options.Gradient.End)
		})
		CircleGradient.Rotation = Options.Gradient.Rotation
        CircleGradient.Parent = CheckedCircle

        local function CheckStatus()
            for _,v in ipairs(SelectGame:GetDescendants()) do
                if v:IsA("UIGradient") then
                    v.Enabled = false
                end

                if v.Name == "CheckedCircle" then
                    TweenObject(v, 0.25, {ImageTransparency = 1})
                end

                if v.Name == "Circle" then
                    TweenObject(v, 0.25, {ImageTransparency = 0})
                end
            end

            TweenObject(Circle, 0.25, {ImageTransparency = 1})
            TweenObject(CheckedCircle, 0.25, {ImageTransparency = 0})

            SelectionGradient.Enabled = true
            CircleGradient.Enabled = true
            WindowData.Selection = Selection_Opt.Script
            TStatus.Text = Translate(("Currently <b>%s</b>"):format(Selection_Opt.Status), Options.Translate)
            ImageLabel.Text = Data.Name or Translate("Unknown", Options.Translate)
            AImage.Image = ("http://www.roblox.com/asset/?id=%s"):format(Data.IconImageAssetId or Data.Image:match("%d+") or "0")
            LastUpdateButton.Text = Translate(("Last Update <b>%s</b>"):format(Selection_Opt.UpdateDate and FormatTime(os.time() - Selection_Opt.UpdateDate) or "Never"), Options.Translate)
            
			if Selection_Opt.Creator then
				CreatorButton.Visible = true
				CreatorButton.Text = Translate(("{Creator%s} <b>%s</b>"):format(Names(Selection_Opt.Creator) > 1 and "s" or "", Selection_Opt.Creator), Options.Translate)
			else
				CreatorButton.Visible = false
			end

			YLoad.Visible = not table.find({"not working", "detected", "under maintenance"}, Selection_Opt.Status:lower())
		end

        if WindowData.Created == 1 then
            CheckStatus()
        end

		SelectionButton.MouseButton1Down:Connect(CheckStatus)
		CheckedCircle.MouseButton1Down:Connect(CheckStatus)
		Circle.MouseButton1Down:Connect(CheckStatus)
    end

    function WindowData:Exit()
        TweenObject(ShadeFrame, 0.25, {BackgroundTransparency = 0})
        TweenObject(Cloud, 0.25, {ImageTransparency = 1})
        TweenObject(MainTitle, 0.25, {TextTransparency = 1})
        task.wait(0.25)
        ContentFrame.Visible = false
        ShadeFrame.Visible = false
        ShadeFrame.BackgroundTransparency = 1
        TweenObject(MainFrame, 0.25, {Size = UDim2.new(0, 624, 0, 76)})
        MainTitle.Position = UDim2.new(0.13533978164196014, 0, 0, 0)
        Cloud.Position = UDim2.new(0.07472220808267593, 0, 0.49460679292678833, 0)
        TweenObject(MainTitle, 0.25, {Size = UDim2.new(0, 193, 0, 76)})
        task.wait(0.25)
        TweenObject(Cloud, 0.25, {ImageTransparency = 0})
        TweenObject(MainTitle, 0.25, {TextTransparency = 0})
        task.wait(1)
        ShadeFrame.Visible = true
        TweenObject(ShadeFrame, 0.25, {BackgroundTransparency = 0})
        task.wait(0.25)
        TweenObject(MainFrame, 0.25, {Size = UDim2.new(0, 0, 0, 0)})
        task.wait(0.25)
        NewInstance:Destroy()
    end

    function WindowData:Initiate()
        ShadeFrame.Visible = true
        ContentFrame.Visible = false

        TweenObject(MainFrame, 0.25, {Size = UDim2.new(0, 624, 0, 76)})
        TweenObject(Cloud, 0.15, {Position = UDim2.new(0.07472220808267593, 0, 0.49460679292678833, 0)})
        TweenObject(MainTitle, 0.25, {Position = UDim2.new(0.13533978164196014, 0, 0, 0)})
        TweenObject(MainTitle, 0.25, {Size = UDim2.new(0, 193, 0, 76)})
        task.wait(0.25)
        TweenObject(ShadeFrame, 0.25, {BackgroundTransparency = 1})
        task.wait(1)
        ContentProvider:PreloadAsync({NewInstance})
        repeat
            task.wait()
        until ContentProvider.RequestQueueSize <= 0
        TweenObject(Splash, 0.5, {Size =  UDim2.new(1, 0, 0, 35)})
        task.wait(0.4)
        TweenObject(Splash, 0.25, {BackgroundTransparency = 1})
        TweenObject(Loader, 0.25, {BackgroundTransparency = 1})
		TweenObject(WelcomeLabel, 0.25, {Visible = false})
        TweenObject(Cloud, 0.15, {Position = UDim2.new(0.093952976167202, 0, 0.09986995160579681, 0)})
        TweenObject(MainTitle, 0.25, {Position = UDim2.new(0.1465577334165573, 0, 0.062476783990859985, 0)})
        TweenObject(MainTitle, 0.25, {Size = UDim2.new(0, 193, 0, 35)})
        TweenObject(MainFrame, 0.25, {Size = UDim2.new(0, 624, 0, 468)})
        task.wait(0.25)
        ShadeFrame.BackgroundTransparency  = 0
        ContentFrame.Visible = true
        TweenObject(ShadeFrame, 0.25, {BackgroundTransparency = 1})
    end

	function WindowData:GetDate(Service, ...)
		local Args = {...}

		Service = CheckType(Service, "string", "local"):lower()

		if Service == "local" then
			Args[1] = CheckType(Args[1], "string", "12/12/1999")

			local Day, Month, Year = Args[1]:match("(%d+)[/-](%d+)[/-](%d+)")

			if not Day then
				Year, Month, Day = Args[1]:match("(%d+)[/-](%d+)[/-](%d+)")
			end

			return os.time({
				year = tonumber(Year),
				month = tonumber(Month),
				day = tonumber(Day),
				hour = 0,
				min = 0
			})
		elseif Service == "github" or Service == "bitbucket" then
			if Args[1] and Args[2] and Args[3] then
				local APIs = {
					Github = ("https://api.github.com/repos/%s/%s/commits?path=%s&per_page=1"):format(Args[1], Args[2], Args[3]),
					Bitbucket = ("https://api.bitbucket.org/2.0/repositories/%s/%s/commits?per_page=1&path=%s"):format(Args[1], Args[2], Args[3])
				}
				if APIs[Service] then
					local Response, Data = pcall(function()
						return HttpService:JSONDecode(game:HttpGet(APIs[Service]))
					end)

					if not Data then
						return false, "No data"
					end

					if not Data["message"] then
						local Year, Month, Day, Hour, Min, Sec

						if Service == "Github" then
							Year, Month, Day, Hour, Min, Sec = Data[1].commit.author.date:match("(%d+)-(%d+)-(%d+)T(%d+):(%d+):(%d+)Z")
						elseif Service == "Bitbucket" then
							Year, Month, Day, Hour, Min, Sec = Data.values[1].date:match("(%d+)-(%d+)-(%d+)T(%d+):(%d+):(%d+)")
						end

						return os.time({
							year = tonumber(Year),
							month = tonumber(Month),
							day = tonumber(Day),
							hour = tonumber(Hour),
							min = tonumber(Min),
							sec = tonumber(Sec)
						})
					else
						return false, Data["message"]
					end

					return false
				end
			end
		end
	end

    function WindowData:TryExecuteURL(URL)
		local Success, Result = pcall(function()
			return loadstring(game:HttpGet(URL), "Loady Script")()
		end)

		return Success
    end

    return WindowData
end

return Library
