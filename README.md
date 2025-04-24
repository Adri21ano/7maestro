local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "OMaestro"
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Função de arrastar elementos
local function makeDraggable(frame)
	local drag, dragInput, start, startPos
	frame.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			drag = true
			start = input.Position
			startPos = frame.Position
			input.Changed:Connect(function()
				if input.UserInputState == Enum.UserInputState.End then
					drag = false
				end
			end)
		end
	end)

	frame.InputChanged:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
			dragInput = input
		end
	end)

	game:GetService("UserInputService").InputChanged:Connect(function(input)
		if input == dragInput and drag then
			local delta = input.Position - start
			frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
		end
	end)
end

-- Tela principal
local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 450, 0, 300)
main.Position = UDim2.new(0.5, -225, 0.5, -150)
main.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
main.BorderSizePixel = 0
main.Visible = true
makeDraggable(main)

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, 0, 0, 40)
title.Text = "O MAESTRO"
title.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
title.Font = Enum.Font.GothamBold
title.TextColor3 = Color3.fromRGB(170, 0, 255)
title.TextScaled = true

-- Ícone flutuante para abrir/fechar o painel
local icon = Instance.new("TextButton", gui)
icon.Size = UDim2.new(0, 50, 0, 50)
icon.Position = UDim2.new(0, 10, 1, -60)
icon.BackgroundColor3 = Color3.fromRGB(170, 0, 255)
icon.Text = "7"
icon.TextColor3 = Color3.fromRGB(255, 255, 255)
icon.Font = Enum.Font.GothamBold
icon.TextScaled = true
icon.BorderSizePixel = 0
makeDraggable(icon)

icon.MouseButton1Click:Connect(function()
	main.Visible = not main.Visible
end)

local autoFarmFrame = Instance.new("Frame", main)
autoFarmFrame.Size = UDim2.new(1, -20, 0, 220)
autoFarmFrame.Position = UDim2.new(0, 10, 0, 60)
autoFarmFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
autoFarmFrame.BorderSizePixel = 0

local autoFarmTitle = Instance.new("TextLabel", autoFarmFrame)
autoFarmTitle.Size = UDim2.new(1, 0, 0, 30)
autoFarmTitle.Position = UDim2.new(0, 0, 0, 0)
autoFarmTitle.Text = "AUTO FARM"
autoFarmTitle.Font = Enum.Font.GothamBold
autoFarmTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
autoFarmTitle.TextScaled = true
autoFarmTitle.BackgroundTransparency = 1

-- Lista de funções e variáveis
local farmList = {
    {label = "Farm Level", var = "_G.FarmLevel"},
    {label = "Farm Boss", var = "_G.FarmBoss"},
    {label = "Farm Espada", var = "_G.FarmEspada"},
    {label = "Farm Arma", var = "_G.FarmArma"},
    {label = "Farm Fruta", var = "_G.FarmFruta"},
    {label = "Auto Baú", var = "_G.AutoBau"},
    {label = "Coletar Drops", var = "_G.AutoDrops"},
}

for i, data in ipairs(farmList) do
    _G[data.var] = false

    local label = Instance.new("TextLabel", autoFarmFrame)
    label.Size = UDim2.new(0.6, 0, 0, 25)
    label.Position = UDim2.new(0, 10, 0, 35 + (i - 1) * 30)
    label.Text = data.label
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.Font = Enum.Font.Gotham
    label.TextScaled = true
    label.BackgroundTransparency = 1
    label.TextXAlignment = Enum.TextXAlignment.Left

    local toggle = Instance.new("Frame", autoFarmFrame)
    toggle.Size = UDim2.new(0, 50, 0, 25)
    toggle.Position = UDim2.new(1, -60, 0, 35 + (i - 1) * 30)
    toggle.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    toggle.BorderSizePixel = 0
    Instance.new("UICorner", toggle).CornerRadius = UDim.new(1, 0)

    local mover = Instance.new("Frame", toggle)
    mover.Size = UDim2.new(0, 23, 0, 23)
    mover.Position = UDim2.new(0, 1, 0, 1)
    mover.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    mover.BorderSizePixel = 0
    Instance.new("UICorner", mover).CornerRadius = UDim.new(1, 0)

    toggle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            _G[data.var] = not _G[data.var]
            toggle.BackgroundColor3 = _G[data.var] and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(100, 100, 100)
            mover:TweenPosition(
                _G[data.var] and UDim2.new(1, -24, 0, 1) or UDim2.new(0, 1, 0, 1),
                Enum.EasingDirection.Out,
                Enum.EasingStyle.Sine,
                0.2,
                true
            )
        end
    end)
end

local runService = game:GetService("RunService")
local pathService = game:GetService("PathfindingService")

-- Auto Clicker suave ativado com Farm Level
spawn(function()
	while task.wait(0.3) do
		if _G.FarmLevel then
			mouse1click()
		end
	end
end)

-- Auto Baú com movimentação real
spawn(function()
	while task.wait(1) do
		if _G.AutoBau then
			local char = player.Character or player.CharacterAdded:Wait()
			local hrp = char:WaitForChild("HumanoidRootPart")
			local humanoid = char:FindFirstChildWhichIsA("Humanoid")

			local closestChest
			local shortestDist = math.huge

			for _, v in pairs(workspace:GetDescendants()) do
				if v:IsA("TouchTransmitter") and v.Parent and (v.Parent.Name == "Chest1" or v.Parent.Name == "Chest2" or v.Parent.Name == "Chest3") then
					local chest = v.Parent
					local dist = (hrp.Position - chest.Position).Magnitude
					if dist < shortestDist then
						shortestDist = dist
						closestChest = chest
					end
				end
			end

			if closestChest then
				local path = pathService:CreatePath({})
				path:ComputeAsync(hrp.Position, closestChest.Position)

				if path.Status == Enum.PathStatus.Complete then
					for _, wp in ipairs(path:GetWaypoints()) do
						if not _G.AutoBau then break end
						humanoid:MoveTo(wp.Position)
						humanoid.MoveToFinished:Wait()
					end
					wait(0.5)
				end
			end
		end
	end
end)

-- Auto Coleta de Drops
spawn(function()
	while task.wait(1) do
		if _G.AutoDrops then
			for _, drop in pairs(workspace:GetDescendants()) do
				if drop:IsA("Tool") or drop:IsA("Model") and drop:FindFirstChild("TouchInterest") then
					local char = player.Character
					if char and char:FindFirstChild("HumanoidRootPart") then
						char.HumanoidRootPart.CFrame = drop:GetModelCFrame() + Vector3.new(0, 2, 0)
						wait(0.3)
					end
				end
			end
		end
	end
end)

-- Farm de Level e Boss (exemplo simplificado)
spawn(function()
	while task.wait() do
		if _G.FarmLevel or _G.FarmBoss then
			for _, mob in pairs(workspace.Enemies:GetChildren()) do
				if mob:FindFirstChild("Humanoid") and mob:FindFirstChild("HumanoidRootPart") and mob.Humanoid.Health > 0 then
					if (_G.FarmLevel and not mob.Name:lower():find("boss")) or (_G.FarmBoss and mob.Name:lower():find("boss")) then
						local char = player.Character or player.CharacterAdded:Wait()
						local hrp = char:WaitForChild("HumanoidRootPart")
						pcall(function()
							hrp.CFrame = mob.HumanoidRootPart.CFrame * CFrame.new(0, 10, 0)
							mouse1click()
						end)
						repeat wait(0.2)
							mouse1click()
						until mob.Humanoid.Health <= 0 or not _G.FarmLevel and not _G.FarmBoss
					end
				end
			end
		end
	end
end)
