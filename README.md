-- Services
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- ScreenGui
local ProjectPP = Instance.new("ScreenGui")
ProjectPP.Name = "Project PP"
ProjectPP.Parent = playerGui
ProjectPP.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ProjectPP.DisplayOrder = 999
ProjectPP.ResetOnSpawn = false

-- ฟังก์ชันสร้าง ImageLabel เป็นวงกลม
local function CreateCircularImage(parent, size, position, zIndex, imageId)
    local img = Instance.new("ImageLabel")
    img.Parent = parent
    img.Size = size
    img.Position = position
    img.ZIndex = zIndex or 1
    img.BackgroundTransparency = 1
    img.Image = "rbxassetid://"..imageId
    img.AnchorPoint = Vector2.new(0.5, 0.5)
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0.5, 0)
    corner.Parent = img
    
    return img
end

-- trajectory ใช้ ID เดิม
local trajectory = CreateCircularImage(ProjectPP, UDim2.new(0, 110, 0, 110), UDim2.new(0.1, 0, 0.8, 0), 1, "7102118272")

-- โลโก้วงกลมใหญ่ซ้อน (R) ใช้ ID ใหม่
local R = CreateCircularImage(ProjectPP, UDim2.new(0, 110, 0, 110), UDim2.new(0.1, 0, 0.8, 0), 3, "96254927950057")

-- ดาวเคราะห์เล็ก (Earth) ใช้ ID ใหม่
local Earth = CreateCircularImage(R, UDim2.new(0, 20, 0, 20), UDim2.new(0.5, 0, 0.5, 0), 4, "96254927950057")

-- เอฟเฟกต์ Glow / Green ใช้ ID ใหม่
local Green = CreateCircularImage(ProjectPP, UDim2.new(0, 110, 0, 110), UDim2.new(0.1, 0, 0.8, 0), 6, "96254927950057")
Green.ImageTransparency = 1

-- Tween โลโก้ใหญ่ไปมุมซ้ายล่างเล็กน้อย
spawn(function()
    while true do
        wait(0.01)
        trajectory.Rotation = trajectory.Rotation + 0.3
    end
end)

-- Tween โลโก้วงกลม R
spawn(function()
    local targetPos = UDim2.new(0.1, 0, 0.8, 0)
    R:TweenPosition(targetPos, "Out", "Sine", 0.4, false)
end)

-- โคจรดาวเคราะห์เล็กรอบ R
spawn(function()
    local angle = 0
    local angleIncrement = 0.02
    local orbitRadius = 55
    while wait() do
        angle = angle + angleIncrement
        local x = math.cos(angle) * orbitRadius
        local y = math.sin(angle) * orbitRadius
        Earth.Position = UDim2.new(0.5, x, 0.5, y)
    end
end)

-- Tween / Fade in/out Green
spawn(function()
    local Tween = TweenService
    wait(2)
    while true do
        local fadeIn = Tween:Create(Green, TweenInfo.new(0.5), {ImageTransparency = 0})
        fadeIn:Play()
        wait(0.3)
        local fadeOut = Tween:Create(Green, TweenInfo.new(0.5), {ImageTransparency = 1})
        fadeOut:Play()
        wait(4)
    end
end)

-- Tween Green ไปมุมซ้ายล่างเล็กน้อย
spawn(function()
    local targetPos = UDim2.new(0.1, 0, 0.8, 0)
    Green:TweenPosition(targetPos, "Out", "Sine", 0.4, false)
end)

print("Loaded At", game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name)
wait(0.1)
print("Welcome,", player.Name)
