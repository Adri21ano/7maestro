
local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui
screenGui.Name = "7maestroGUI"

local frame = Instance.new("Frame")
frame.Parent = screenGui
frame.Size = UDim2.new(0, 500, 0, 500)  -- Ajustando o tamanho do frame para um layout mais adequado
frame.Position = UDim2.new(0.5, -250, 0.5, -250)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BorderSizePixel = 0
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.BackgroundTransparency = 0.2
frame.Visible = false  -- Inicialmente a tela está oculta

function createButton(text, position, callback)
    local button = Instance.new("TextButton")
    button.Parent = frame
    button.Size = UDim2.new(0, 460, 0, 40)  -- Botões com tamanho compacto
    button.Position = position
    button.BackgroundColor3 = Color3.fromRGB(255, 85, 85)
    button.Text = text
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 18
    button.TextButton.MouseButton1Click:Connect(callback)
    
    button.MouseEnter:Connect(function()
        button.BackgroundColor3 = Color3.fromRGB(255, 170, 0)
    end)
    button.MouseLeave:Connect(function()
        button.BackgroundColor3 = Color3.fromRGB(255, 85, 85)
    end)

    return button
end

createButton("Farm de Mundo 1", UDim2.new(0, 20, 0, 70), function()
    print("Iniciando farm no Mundo 1.")
    -- Função para farm no Mundo 1
end)

createButton("Farm de Mundo 2", UDim2.new(0, 20, 0, 120), function()
    print("Iniciando farm no Mundo 2.")
    -- Função para farm no Mundo 2
end)

createButton("Farm de Mundo 3", UDim2.new(0, 20, 0, 170), function()
    print("Iniciando farm no Mundo 3.")
    -- Função para farm no Mundo 3
end)

createButton("Farm de Mundo 4", UDim2.new(0, 20, 0, 220), function()
    print("Iniciando farm no Mundo 4.")
    -- Função para farm no Mundo 4
end)

createButton("Farm de Bosses", UDim2.new(0, 20, 0, 270), function()
    print("Iniciando farm de bosses.")
    -- Função para farm de bosses
end)

local iconFrame = Instance.new("Frame")
iconFrame.Parent = screenGui
iconFrame.Size = UDim2.new(0, 50, 0, 50)
iconFrame.Position = UDim2.new(0.95, -55, 0.95, -55)
iconFrame.BackgroundColor3 = Color3.fromRGB(255, 0, 255)
iconFrame.BorderSizePixel = 0
iconFrame.AnchorPoint = Vector2.new(0.5, 0.5)

-- Ícone com o número 7
local iconLabel = Instance.new("TextLabel")
iconLabel.Parent = iconFrame
iconLabel.Size = UDim2.new(1, 0, 1, 0)
iconLabel.Text = "7"
iconLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
iconLabel.TextSize = 30
iconLabel.TextAlign = Enum.TextXAlignment.Center
iconLabel.TextYAlignment = Enum.TextYAlignment.Center
iconLabel.BackgroundTransparency = 1

-- Lógica para alternar entre mostrar e ocultar a tela de início
local screenVisible = false
iconFrame.MouseButton1Click:Connect(function()
    screenVisible = not screenVisible
    frame.Visible = screenVisible
end)

-- Funcionalidade de arrastar a tela
local dragging = false
local dragInput
local dragStart
local startPos

frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = frame.Position
    end
end)

frame.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.Touch then
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

frame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end)
