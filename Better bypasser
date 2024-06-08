local executorEnviroment = debug.getfenv()
local Services = setmetatable({}, {
	__index = function(_, serviceName)
		if debug.getfenv(2) ~= executorEnviroment then
			return nil;
		end
		return cloneref(game[serviceName])
	end
})

local player = Services.Players.LocalPlayer
repeat task.wait() until game:IsLoaded() and player.Character

getgenv().options = ({...})[1]

local success, error = pcall(function()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/Synergy-Networks/products/main/BetterBypasser/publicproduct.lua",true))()
end)

if not success and error then
	print("A error occured while trying to launch the main script. Error: " .. error)
end
