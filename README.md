local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

local mainImageId = "rbxassetid://74274591652009"

-- สร้าง ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AnimatedImageGui"
screenGui.Parent = playerGui

-- ขนาดและรัศมีวงแหวน
local mainSize = 250      -- ขนาดภาพใหญ่
local starSize = 50       -- ขนาดดาวรอบ
local numStars = 6        -- จำนวนดาวรอบ
local radius = 80         -- รัศมีวงแหวน

-- จุดตำแหน่ง
local screenCenter = UDim2.new(0.5, 0, 0.5, 0)         -- กลางหน้าจอ
local topLeft = UDim2.new(0, radius, 0, radius)       -- มุมซ้ายบน + offset

-- สร้างภาพใหญ่ตรงกลาง
local mainImage = Instance.new("ImageLabel")
mainImage.Size = UDim2.new(0, mainSize, 0, mainSize)
mainImage.Position = screenCenter
mainImage.AnchorPoint = Vector2.new(0.5, 0.5)
mainImage.BackgroundTransparency = 1
mainImage.Image = mainImageId
mainImage.ScaleType = Enum.ScaleType.Fit
mainImage.Parent = screenGui

-- สร้างดาวรอบวงแหวน
local stars = {}
for i = 1, numStars do
    local star = Instance.new("ImageLabel")
    star.Size = UDim2.new(0, starSize, 0, starSize)
    star.BackgroundTransparency = 1
    star.Image = mainImageId
    star.ScaleType = Enum.ScaleType.Fit
    star.AnchorPoint = Vector2.new(0.5, 0.5)
    star.Position = screenCenter
    star.Parent = screenGui
    table.insert(stars, {obj = star, angle = (360/numStars)*(i-1)})
end

-- Tween ให้ภาพใหญ่เคลื่อนจากกลางไปมุมซ้ายบน
TweenService:Create(mainImage, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = topLeft}):Play()
for _, starData in pairs(stars) do
    TweenService:Create(starData.obj, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Position = topLeft}):Play()
end

-- ความเร็วหมุน
local mainRotationSpeed = 30
local ringRotationSpeed = 60

-- หมุนภาพใหญ่และดาวรอบวงแหวน
RunService.Heartbeat:Connect(function(dt)
    -- หมุนภาพใหญ่
    mainImage.Rotation = (mainImage.Rotation + mainRotationSpeed * dt) % 360

    -- คำนวณจุดศูนย์กลางของภาพใหญ่
    local centerX = mainImage.AbsolutePosition.X + mainSize/2
    local centerY = mainImage.AbsolutePosition.Y + mainSize/2

    -- หมุนดาวรอบวงแหวน
    for _, starData in pairs(stars) do
        starData.angle = (starData.angle + ringRotationSpeed * dt) % 360
        local rad = math.rad(starData.angle)
        starData.obj.Position = UDim2.new(0, centerX + radius*math.cos(rad), 0, centerY + radius*math.sin(rad))
    end
end)
