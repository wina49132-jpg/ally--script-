-- [ Shadow - Universal Fly Script with GUI ] --
local p = game.Players.LocalPlayer
local c = p.Character or p.CharacterAdded:Wait()
local hrp = c:WaitForChild("HumanoidRootPart")
local uis = game:GetService("UserInputService")
local rs = game:GetService("RunService")
local flying, speed = false, 50
local bv, bg

local sg = Instance.new("ScreenGui", p.PlayerGui)
sg.Name = "ShadowFlyGUI"
sg.ResetOnSpawn = false

local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 120, 0, 50)
btn.Position = UDim2.new(0.1, 0, 0.1, 0)
btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.TextSize = 18
btn.Font = Enum.Font.SourceSansBold
btn.Text = "Fly: OFF"
btn.Active = true
btn.Draggable = true

local function toggleFly()
    flying = not flying
    if flying then
        btn.Text = "Fly: ON"
        btn.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
        bv = Instance.new("BodyVelocity", hrp)
        bv.MaxForce = Vector3.new(1e9, 1e9, 1e9)
        bv.Velocity = Vector3.zero
        bg = Instance.new("BodyGyro", hrp)
        bg.MaxTorque = Vector3.new(1e9, 1e9, 1e9)
        bg.CFrame = hrp.CFrame
        c.Humanoid.PlatformStand = true
    else
        btn.Text = "Fly: OFF"
        btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        if bv then bv:Destroy() end
        if bg then bg:Destroy() end
        c.Humanoid.PlatformStand = false
    end
end

btn.MouseButton1Click:Connect(toggleFly)

uis.InputBegan:Connect(function(i, gp)
    if gp then return end
    if i.KeyCode == Enum.KeyCode.F then
        toggleFly()
    end
end)

rs.RenderStepped:Connect(function()
    if flying and bv and bg then
        local cam = workspace.CurrentCamera
        local md = Vector3.zero
        if uis:IsKeyDown(Enum.KeyCode.W) then md = md + cam.CFrame.LookVector end
        if uis:IsKeyDown(Enum.KeyCode.S) then md = md - cam.CFrame.LookVector end
        if uis:IsKeyDown(Enum.KeyCode.A) then md = md - cam.CFrame.RightVector end
        if uis:IsKeyDown(Enum.KeyCode.D) then md = md + cam.CFrame.RightVector end
        bv.Velocity = md * speed
        bg.CFrame = cam.CFrame
    end
end)
