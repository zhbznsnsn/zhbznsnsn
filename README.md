local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local camera = game.Workspace.CurrentCamera

-- GUI
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "Neuz"
gui.ResetOnSpawn = false

-- SETTINGS
local camlockOn = false
local prediction = 0.13
local smoothness = 0.06
local jumpOffset = 0.12
local autoAir = false
local airDelay = 0.2
local lastShot = 0
local currentTarget = nil

-- GUI BUTTONS
local function createButton(text, size, pos)
	local btn = Instance.new("TextButton")
	btn.Size = size
	btn.Position = pos
	btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	btn.Text = text
	btn.TextColor3 = Color3.new(1,1,1)
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 16
	btn.Draggable = true
	Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
	btn.Parent = gui
	return btn
end

local camlockButton = createButton("Camlock: OFF", UDim2.new(0, 120, 0, 40), UDim2.new(0, 10, 0.35, 0))
local autoAirButton = createButton("Auto Air: OFF", UDim2.new(0, 120, 0, 30), UDim2.new(0, 10, 0.43, 0))
local toggleButton = createButton("neuz.cc", UDim2.new(0, 100, 0, 35), UDim2.new(0, 10, 0.5, 0))

-- PANEL
local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 500, 0, 300)
main.Position = UDim2.new(0.5, -250, 0.5, -150)
main.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
main.Visible = false
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)

-- TOGGLE LOGIC
camlockButton.MouseButton1Click:Connect(function()
	camlockOn = not camlockOn
	camlockButton.Text = "Camlock: " .. (camlockOn and "ON" or "OFF")
	currentTarget = camlockOn and getNearestPlayer() or nil
end)

autoAirButton.MouseButton1Click:Connect(function()
	autoAir = not autoAir
	autoAirButton.Text = "Auto Air: " .. (autoAir and "ON" or "OFF")
end)

toggleButton.MouseButton1Click:Connect(function()
	main.Visible = not main.Visible
end)

-- SETTINGS INPUTS
local function createSetting(name, default, position, callback)
	local label = Instance.new("TextLabel", main)
	label.Text = name
	label.Size = UDim2.new(0, 100, 0, 25)
	label.Position = position
	label.TextColor3 = Color3.new(1, 1, 1)
	label.BackgroundTransparency = 1
	label.Font = Enum.Font.Gotham
	label.TextSize = 14

	local box = Instance.new("TextBox", main)
	box.Size = UDim2.new(0, 100, 0, 25)
	box.Position = position + UDim2.new(0, 110, 0, 0)
	box.Text = tostring(default)
	box.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
	box.TextColor3 = Color3.new(1, 1, 1)
	box.Font = Enum.Font.Gotham
	box.TextSize = 14
	Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)

	box.FocusLost:Connect(function()
		local val = tonumber(box.Text)
		if val then
			callback(val)
			box.Text = tostring(val)
		else
			box.Text = tostring(default)
		end
	end)
end

-- Create Settings (adjustable values)
createSetting("Prediction", prediction, UDim2.new(0, 30, 0, 30), function(val)
	prediction = val
end)

createSetting("Smoothness", smoothness, UDim2.new(0, 30, 0, 70), function(val)
	smoothness = val
end)

createSetting("JumpOffset", jumpOffset, UDim2.new(0, 30, 0, 110), function(val)
	jumpOffset = val
end)

createSetting("AirDelay", airDelay, UDim2.new(0, 30, 0, 150), function(val)
	airDelay = val
end)

-- GET NEAREST PLAYER
function getNearestPlayer()
	local nearest, shortest = nil, math.huge
	for _, v in ipairs(game.Players:GetPlayers()) do
		if v ~= player and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
			local screenPos, visible = camera:WorldToViewportPoint(v.Character.HumanoidRootPart.Position)
			if visible then
				local dist = (Vector2.new(mouse.X, mouse.Y) - Vector2.new(screenPos.X, screenPos.Y)).Magnitude
				if dist < shortest then
					shortest = dist
					nearest = v
				end
			end
		end
	end
	return nearest
end

-- RENDERSTEPPED LOOP
RunService.RenderStepped:Connect(function()
	if camlockOn and currentTarget and currentTarget.Character and currentTarget.Character:FindFirstChild("HumanoidRootPart") then
		local hrp = currentTarget.Character.HumanoidRootPart
		local velocity = hrp.Velocity
		local predicted = hrp.Position + velocity * prediction

		-- Vertical offset if in air
		if not currentTarget.Character:FindFirstChildOfClass("Humanoid").FloorMaterial.Name ~= "Air" then
			predicted = predicted + Vector3.new(0, jumpOffset, 0)
		end

		-- Smooth camera aim
		local lookVector = (predicted - camera.CFrame.Position).Unit
		local targetCFrame = CFrame.new(camera.CFrame.Position, camera.CFrame.Position + lookVector)
		camera.CFrame = camera.CFrame:Lerp(targetCFrame, smoothness)
	end
end)local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local camera = game.Workspace.CurrentCamera

-- GUI
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "Neuz"
gui.ResetOnSpawn = false

-- SETTINGS
local camlockOn = false
local prediction = 0.13
local smoothness = 0.06
local jumpOffset = 0.12
local autoAir = false
local airDelay = 0.2
local lastShot = 0
local currentTarget = nil

-- GUI BUTTONS
local function createButton(text, size, pos)
	local btn = Instance.new("TextButton")
	btn.Size = size
	btn.Position = pos
	btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	btn.Text = text
	btn.TextColor3 = Color3.new(1,1,1)
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 16
	btn.Draggable = true
	Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
	btn.Parent = gui
	return btn
end

local camlockButton = createButton("Camlock: OFF", UDim2.new(0, 120, 0, 40), UDim2.new(0, 10, 0.35, 0))
local autoAirButton = createButton("Auto Air: OFF", UDim2.new(0, 120, 0, 30), UDim2.new(0, 10, 0.43, 0))
local toggleButton = createButton("neuz.cc", UDim2.new(0, 100, 0, 35), UDim2.new(0, 10, 0.5, 0))

-- PANEL
local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 500, 0, 300)
main.Position = UDim2.new(0.5, -250, 0.5, -150)
main.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
main.Visible = false
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)

-- TOGGLE LOGIC
camlockButton.MouseButton1Click:Connect(function()
	camlockOn = not camlockOn
	camlockButton.Text = "Camlock: " .. (camlockOn and "ON" or "OFF")
	currentTarget = camlockOn and getNearestPlayer() or nil
end)

autoAirButton.MouseButton1Click:Connect(function()
	autoAir = not autoAir
	autoAirButton.Text = "Auto Air: " .. (autoAir and "ON" or "OFF")
end)

toggleButton.MouseButton1Click:Connect(function()
	main.Visible = not main.Visible
end)

-- SETTINGS INPUTS
local function createSetting(name, default, position, callback)
	local label = Instance.new("TextLabel", main)
	label.Text = name
	label.Size = UDim2.new(0, 100, 0, 25)
	label.Position = position
	label.TextColor3 = Color3.new(1, 1, 1)
	label.BackgroundTransparency = 1
	label.Font = Enum.Font.Gotham
	label.TextSize = 14

	local box = Instance.new("TextBox", main)
	box.Size = UDim2.new(0, 100, 0, 25)
	box.Position = position + UDim2.new(0, 110, 0, 0)
	box.Text = tostring(default)
	box.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
	box.TextColor3 = Color3.new(1, 1, 1)
	box.Font = Enum.Font.Gotham
	box.TextSize = 14
	Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)

	box.FocusLost:Connect(function()
		local val = tonumber(box.Text)
		if val then
			callback(val)
			box.Text = tostring(val)
		else
			box.Text = tostring(default)
		end
	end)
end

-- Create Settings (adjustable values)
createSetting("Prediction", prediction, UDim2.new(0, 30, 0, 30), function(val)
	prediction = val
end)

createSetting("Smoothness", smoothness, UDim2.new(0, 30, 0, 70), function(val)
	smoothness = val
end)

createSetting("JumpOffset", jumpOffset, UDim2.new(0, 30, 0, 110), function(val)
	jumpOffset = val
end)

createSetting("AirDelay", airDelay, UDim2.new(0, 30, 0, 150), function(val)
	airDelay = val
end)

-- GET NEAREST PLAYER
function getNearestPlayer()
	local nearest, shortest = nil, math.huge
	for _, v in ipairs(game.Players:GetPlayers()) do
		if v ~= player and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
			local screenPos, visible = camera:WorldToViewportPoint(v.Character.HumanoidRootPart.Position)
			if visible then
				local dist = (Vector2.new(mouse.X, mouse.Y) - Vector2.new(screenPos.X, screenPos.Y)).Magnitude
				if dist < shortest then
					shortest = dist
					nearest = v
				end
			end
		end
	end
	return nearest
end

-- RENDERSTEPPED LOOP
RunService.RenderStepped:Connect(function()
	if camlockOn and currentTarget and currentTarget.Character and currentTarget.Character:FindFirstChild("HumanoidRootPart") then
		local hrp = currentTarget.Character.HumanoidRootPart
		local velocity = hrp.Velocity
		local predicted = hrp.Position + velocity * prediction

		-- Vertical offset if in air
		if not currentTarget.Character:FindFirstChildOfClass("Humanoid").FloorMaterial.Name ~= "Air" then
			predicted = predicted + Vector3.new(0, jumpOffset, 0)
		end

		-- Smooth camera aim
		local lookVector = (predicted - camera.CFrame.Position).Unit
		local targetCFrame = CFrame.new(camera.CFrame.Position, camera.CFrame.Position + lookVector)
		camera.CFrame = camera.CFrame:Lerp(targetCFrame, smoothness)
	end
end)
