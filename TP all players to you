local plr = game:GetService("Players").LocalPlayer
for i,v in pairs(game:GetService("Players"):GetChildren()) do
    if v.ClassName == "Player" and v.Name ~= plr.Name then
        if v.Character:FindFirstChild("HumanoidRootPart") then
            v.Character.HumanoidRootPart.CFrame = CFrame.new(plr.Character.HumanoidRootPart.Position)
        end
    end
end
