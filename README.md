local COMMAND = "меч"
local DEFAULT_COLOR = Color3.fromRGB(0, 100, 255)


local screenGui = Instance.new("ScreenGui")
screenGui.Name = "LightsaberUI"
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")


local toggleBtn = Instance.new("TextButton")
toggleBtn.Name = "ToggleBtn"
toggleBtn.Size = UDim2.new(0.15, 0, 0.08, 0)
toggleBtn.Position = UDim2.new(0.83, 0, 0.7, 0)
toggleBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
toggleBtn.TextColor3 = Color3.new(1, 1, 1)
toggleBtn.Text = "Вкл/Выкл"
toggleBtn.Parent = screenGui


local colorBtn = Instance.new("TextButton")
colorBtn.Name = "ColorBtn"
colorBtn.Size = UDim2.new(0.15, 0, 0.08, 0)
colorBtn.Position = UDim2.new(0.83, 0, 0.8, 0)
colorBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
colorBtn.TextColor3 = Color3.new(1, 1, 1)
colorBtn.Text = "Цвет"
colorBtn.Parent = screenGui

local tool, blade, light
local activated = true
local currentColor = DEFAULT_COLOR

local function createHilt()
    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(0.6, 1.2, 0.6)
    handle.Color = Color3.fromRGB(40, 40, 40)
    handle.Material = Enum.Material.Metal
    
    
    local parts = {
        {Size = Vector3.new(0.7, 0.15, 0.7), Color = Color3.fromRGB(100, 100, 100), Offset = Vector3.new(0, 0.7, 0)},
        {Size = Vector3.new(0.5, 0.8, 0.5), Color = Color3.fromRGB(30, 30, 30), Offset = Vector3.new(0, 0, 0)},
        {Size = Vector3.new(0.3, 0.1, 0.3), Color = Color3.fromRGB(200, 0, 0), Offset = Vector3.new(0.3, 0.3, 0)}
    }
    
    for _, partData in ipairs(parts) do
        local part = Instance.new("Part")
        part.Size = partData.Size
        part.Color = partData.Color
        part.Material = Enum.Material.Metal
        part.Anchored = false
        part.CanCollide = false
        
        local weld = Instance.new("WeldConstraint")
        weld.Part0 = handle
        weld.Part1 = part
        weld.Parent = part
        
        part.Position = handle.Position + partData.Offset
        part.Parent = handle.Parent
    end
    
    return handle
end

local function toggleBlade()
    activated = not activated
    if activated then
        blade.Transparency = 0.2
        light.Enabled = true
        toggleBtn.Text = "Выключить"
    else
        blade.Transparency = 1
        light.Enabled = false
        toggleBtn.Text = "Включить"
    end
end

local function changeColor()
    local colors = {
        Color3.fromRGB(0, 100, 255), -- Синий
        Color3.fromRGB(255, 0, 0),   -- Красный
        Color3.fromRGB(0, 255, 0),   -- Зеленый
        Color3.fromRGB(255, 255, 0)  -- Желтый
    }
    
    currentColor = colors[((table.find(colors, currentColor) or 0) % #colors) + 1]
    blade.Color = currentColor
    light.Color = currentColor
end

local function createLightsaber()
    local character = game.Players.LocalPlayer.Character
    if not character then return end

    
    for _, item in ipairs(character:GetChildren()) do
        if item:IsA("Tool") then item:Destroy() end
    end

    
    tool = Instance.new("Tool")
    tool.Name = "Lightsaber"
    tool.Parent = character

    
    tool.GripPos = Vector3.new(0, -0.5, 0)
    tool.GripForward = Vector3.new(0, 0, -1)
    tool.GripRight = Vector3.new(1, 0, 0)
    tool.GripUp = Vector3.new(0, 1, 0)

    
    local handle = createHilt()
    handle.Parent = tool

    
    blade = Instance.new("Part")
    blade.Name = "Blade"
    blade.Size = Vector3.new(0.7, 5, 0.7)
    blade.Color = currentColor
    blade.Material = Enum.Material.Neon
    blade.Transparency = 0.2
    blade.CanCollide = true
    blade.Parent = tool

    
    local weld = Instance.new("WeldConstraint")
    weld.Part0 = handle
    weld.Part1 = blade
    weld.Parent = blade
    blade.Position = handle.Position + Vector3.new(0, 3.5, 0)

   

    light = Instance.new("PointLight")
    light.Color = currentColor
    light.Brightness = 10
    light.Range = 12
    light.Parent = blade

    
    toggleBtn.MouseButton1Click:Connect(toggleBlade)
    colorBtn.MouseButton1Click:Connect(changeColor)

    
    tool.Parent = character
    tool:Activate()
end

game.Players.LocalPlayer.Chatted:Connect(function(msg)
    if msg:lower() == COMMAND then
        createLightsaber()
    end
end)

Напишите "меч" для получения меча 
