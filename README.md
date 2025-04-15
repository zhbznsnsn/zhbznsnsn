-- UI Elements
local gui = Instance.new("ScreenGui", game.CoreGui)
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 200, 0, 220)
frame.Position = UDim2.new(0.05, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
frame.Active = true
frame.Draggable = true

-- Variables for multiplier
local wsBoost = 16
local jpBoost = 50

-- Walkspeed Button
local wsBtn = Instance.new("TextButton", frame)
wsBtn.Size = UDim2.new(1, -20, 0, 40)
wsBtn.Position = UDim2.new(0, 10, 0, 10)
wsBtn.Text = "WalkSpeed Boost"
wsBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
wsBtn.MouseButton1Click:Connect(function()
    wsBoost = wsBoost + 10
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = wsBoost
end)

-- JumpPower Button
local jpBtn = Instance.new("TextButton", frame)
jpBtn.Size = UDim2.new(1, -20, 0, 40)
jpBtn.Position = UDim2.new(0, 10, 0, 60)
jpBtn.Text = "JumpPower Boost"
jpBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
jpBtn.MouseButton1Click:Connect(function()
    jpBoost = jpBoost + 25
    game.Players.LocalPlayer.Character.Humanoid.JumpPower = jpBoost
end)

-- Infinite Stamina Button (Multiplies when clicked)
local stamBtn = Instance.new("TextButton", frame)
stamBtn.Size = UDim2.new(1, -20, 0, 40)
stamBtn.Position = UDim2.new(0, 10, 0, 110)
stamBtn.Text = "Infinite Stamina"
stamBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
stamBtn.MouseButton1Click:Connect(function()
    local char = game.Players.LocalPlayer.Character
    local stam = char:FindFirstChild("Stamina") or char:WaitForChild("Stamina")
    if stam then
        stam:GetPropertyChangedSignal("Value"):Connect(function()
            stam.Value = stam.MaxValue or 9999
        end)
        stam.Value = stam.MaxValue or 9999
    end
end)

-- No Damage / Godmode Button
local godBtn = Instance.new("TextButton", frame)
godBtn.Size = UDim2.new(1, -20, 0, 40)
godBtn.Position = UDim2.new(0, 10, 0, 160)
godBtn.Text = "No Damage (Godmode)"
godBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
godBtn.MouseButton1Click:Connect(function()
    local player = game.Players.LocalPlayer
    local char = player.Character or player.CharacterAdded:Wait()
    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum then
        hum:GetPropertyChangedSignal("Health"):Connect(function()
            hum.Health = hum.MaxHealth
        end)
        hum.Health = hum.MaxHealth
    end
end)
