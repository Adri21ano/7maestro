-- Criação da GUI do Script
local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui
screenGui.Name = "7maestroGUI"

-- Frame principal (Tela de início)
local frame = Instance.new("Frame")
frame.Parent = screenGui
frame.Size = UDim2.new(0, 500, 0, 500)  -- Ajustando o tamanho do frame para um layout mais adequado para celular
frame.Position = UDim2.new(0.5, -250, 0.5, -250)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BorderSizePixel = 0
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.BackgroundTransparency = 0.2

-- Título do script
local title = Instance.new("TextLabel")
title.Parent = frame
title.Size = UDim2.new(1, 0, 0, 50)
title.BackgroundTransparency = 1
title.Text = "7maestro - Blox Fruits"
title.TextColor3 = Color3.fromRGB(255, 0, 0)  -- Cor do título (vermelho)
title.TextSize = 30
title.TextAlign = Enum.TextXAlignment.Center
title.Font = Enum.Font.GothamBold
title.TextStrokeTransparency = 0.8
title.TextStrokeColor3 = Color3.fromRGB(255, 255, 255)

-- Função para criar botões
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
    
    -- Efeito de animação no botão (ao passar o mouse)
    button.MouseEnter:Connect(function()
        button.BackgroundColor3 = Color3.fromRGB(255, 170, 0)
    end)
    button.MouseLeave:Connect(function()
        button.BackgroundColor3 = Color3.fromRGB(255, 85, 85)
    end)

    return button
end

-- Criando os botões (distribuídos de forma mais compacta)
createButton("Ativar Auto Farm de Bosses", UDim2.new(0, 20, 0, 70), function()
    print("Auto Farm de Bosses ativado.")
    AutoFarmBoss()
end)

createButton("Ativar Auto Haki", UDim2.new(0, 20, 0, 120), function()
    print("Auto Haki ativado.")
    AtivarHaki()
end)

createButton("Ativar Auto Coleta de Itens", UDim2.new(0, 20, 0, 170), function()
    print("Auto Coleta de Itens ativado.")
    AutoFarmItens()
end)

createButton("Ativar Auto Quest", UDim2.new(0, 20, 0, 220), function()
    print("Auto Quest ativado.")
    AutoQuest()
end)

createButton("Escolher Ilha", UDim2.new(0, 20, 0, 270), function()
    print("Escolher Ilha ativado.")
    EscolherIlha()
end)

-- Funções automáticas de exemplo (não implementadas, você pode adaptar)
function AtivarHaki()
    -- Função que ativa o Haki
    print("Haki ativado!")
end

function AutoFarmBoss()
    -- Função para auto farm de bosses
    print("Auto Farm de Bosses ativado!")
end

function AutoFarmItens()
    -- Função para auto coleta de itens
    print("Auto Coleta de Itens ativado!")
end

function AutoQuest()
    -- Função para auto aceitação de quests
    print("Auto Quest ativado!")
end

function EscolherIlha()
    -- Função de teleporte entre ilhas
    print("Escolher Ilha ativado!")
end

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
