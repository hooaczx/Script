-- Variables
local speedValue = 16 -- Default speed value

-- Create GUI
local gui = Instance.new("ScreenGui")
gui.Name = "JN HH Gaming"
gui.Parent = game.Players.LocalPlayer.PlayerGui

local frame = Instance.new("Frame")
frame.Name = "SpeedFrame"
frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0, 10, 0, 10)
frame.BackgroundColor3 = Color3.new(0, 0, 0)
frame.BackgroundTransparency = 0.5
frame.Parent = gui

local speedLabel = Instance.new("TextLabel")
speedLabel.Name = "SpeedLabel"
speedLabel.Size = UDim2.new(0, 180, 0, 30)
speedLabel.Position = UDim2.new(0, 10, 0, 10)
speedLabel.BackgroundColor3 = Color3.new(0, 0, 0)
speedLabel.TextColor3 = Color3.new(1, 1, 1)
speedLabel.TextSize = 18
speedLabel.Text = "Speed: " .. speedValue
speedLabel.Parent = frame

local decreaseButton = Instance.new("TextButton")
decreaseButton.Name = "DecreaseButton"
decreaseButton.Size = UDim2.new(0, 50, 0, 30)
decreaseButton.Position = UDim2.new(0, 10, 0, 50)
decreaseButton.BackgroundColor3 = Color3.new(0, 1, 0)
decreaseButton.TextColor3 = Color3.new(1, 1, 1)
decreaseButton.TextSize = 14
decreaseButton.Text = "-"
decreaseButton.Parent = frame

local increaseButton = Instance.new("TextButton")
increaseButton.Name = "IncreaseButton"
increaseButton.Size = UDim2.new(0, 50, 0, 30)
increaseButton.Position = UDim2.new(0, 140, 0, 50)
increaseButton.BackgroundColor3 = Color3.new(0, 1, 0)
increaseButton.TextColor3 = Color3.new(1, 1, 1)
increaseButton.TextSize = 14
increaseButton.Text = "+"
increaseButton.Parent = frame

-- Functions
local function updateSpeedLabel()
    speedLabel.Text = "Speed: " .. speedValue
end

local function decreaseSpeed()
    if speedValue > 1 then
        speedValue = speedValue - 1
        updateSpeedLabel()
    end
end

local function increaseSpeed()
    speedValue = speedValue + 1
    updateSpeedLabel()
end

local function onDecreaseButtonClicked()
    decreaseSpeed()
end

local function onIncreaseButtonClicked()
    increaseSpeed()
end

-- Event connections
decreaseButton.MouseButton1Click:Connect(onDecreaseButtonClicked)
increaseButton.MouseButton1Click:Connect(onIncreaseButtonClicked)

-- Main loop
while true do
    -- Modify the speed of the character (you need to replace this with the appropriate code for your specific game)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = speedValue
    wait(0.1) -- Adjust the wait time as desired
end
