local player = game.Players.LocalPlayer

local playerGui = player:WaitForChild("PlayerGui")

local RunService = game:GetService("RunService")

local mainImageId = "rbxassetid://74274591652009"

-- สร้าง ScreenGui

local screenGui = Instance.new("ScreenGui")

screenGui.Name = "AnimatedImageGui"

screenGui.Parent = playerGui

-- ขนาด

local mainSize = 250

local starSize = 50

local numStars = 6

local radius = 150 -- รัศมีวงแหวน

-- สร้างภาพใหญ่ตรงกลางวงแหวน

local mainImage = Instance.new("ImageLabel")

mainImage.Size = UDim2.new(0, mainSize, 0, mainSize)

mainImage.Position = UDim2.new(0, radius, 0, radius) -- มุมซ้ายบน + offset ให้วงแหวนพอดี

mainImage.AnchorPoint = Vector2.new(0.5, 0.5) -- จุดศูนย์กลางของภาพใหญ่

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

    star.AnchorPoint = Vector2.new(0.5, 0.5) -- จุดศูนย์กลางของดาว

    star.Parent = screenGui

    table.insert(stars, {obj = star, angle = (360/numStars)*(i-1)})

end

-- หมุนภาพใหญ่และดาวรอบวงแหวน

local mainRotationSpeed = 30

local ringRotationSpeed = 60

RunService.Heartbeat:Connect(function(dt)

    -- หมุนภาพใหญ่

    mainImage.Rotation = (mainImage.Rotation + mainRotationSpeed * dt) % 360

    -- คำนวณจุดศูนย์กลางของภาพใหญ่ (และวงแหวน)

    local centerX = mainImage.AbsolutePosition.X + mainSize/2

    local centerY = mainImage.AbsolutePosition.Y + mainSize/2

    -- หมุนดาวรอบวงแหวน

    for _, starData in pairs(stars) do

        starData.angle = (starData.angle + ringRotationSpeed * dt) % 360

        local rad = math.rad(starData.angle)

        starData.obj.Position = UDim2.new(0, centerX + radius*math.cos(rad), 0, centerY + radius*math.sin(rad))

    end

end)
