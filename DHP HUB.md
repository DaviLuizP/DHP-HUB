local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

-- GUI
local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "DHP_HUB"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 300, 0, 240)
frame.Position = UDim2.new(0.5, -150, 0.5, -120)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

-- Borda vermelha
local stroke = Instance.new("UIStroke", frame)
stroke.Color = Color3.fromRGB(200, 0, 0)
stroke.Thickness = 3
stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

-- Título com efeito rainbow
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.Text = "DHP HUB"
title.BackgroundTransparency = 1
title.Font = Enum.Font.SourceSansBold
title.TextScaled = true
title.Parent = frame

local hue = 0
RunService.RenderStepped:Connect(function()
	hue = hue + 0.01
	if hue > 1 then hue = 0 end
	local rainbow = Color3.fromHSV(hue, 1, 1)
	title.TextColor3 = rainbow
end)

-- Botão Set Speed
local speedBtn = Instance.new("TextButton", frame)
speedBtn.Size = UDim2.new(0.9, 0, 0, 30)
speedBtn.Position = UDim2.new(0.05, 0, 0, 50)
speedBtn.Text = "Setar Velocidade"
speedBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
speedBtn.TextColor3 = Color3.fromRGB(255, 0, 0)
speedBtn.Font = Enum.Font.SourceSansBold
speedBtn.TextScaled = true
speedBtn.Parent = frame

speedBtn.MouseButton1Click:Connect(function()
	local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
	if humanoid then
		humanoid.WalkSpeed = 100 -- você pode mudar esse valor
	end
end)

-- Botão Infinite Jump
local jumpBtn = Instance.new("TextButton", frame)
jumpBtn.Size = UDim2.new(0.9, 0, 0, 30)
jumpBtn.Position = UDim2.new(0.05, 0, 0, 90)
jumpBtn.Text = "Ativar Infinite Jump"
jumpBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
jumpBtn.TextColor3 = Color3.fromRGB(255, 0, 0)
jumpBtn.Font = Enum.Font.SourceSansBold
jumpBtn.TextScaled = true
jumpBtn.Parent = frame

local infiniteJumpAtivo = false
jumpBtn.MouseButton1Click:Connect(function()
	infiniteJumpAtivo = not infiniteJumpAtivo
	jumpBtn.Text = infiniteJumpAtivo and "Desativar Infinite Jump" or "Ativar Infinite Jump"
end)

UserInputService.JumpRequest:Connect(function()
	if infiniteJumpAtivo and LocalPlayer.Character then
		local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
		if humanoid then
			humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
		end
	end
end)

-- Botão Noclip
local noclipBtn = Instance.new("TextButton", frame)
noclipBtn.Size = UDim2.new(0.9, 0, 0, 30)
noclipBtn.Position = UDim2.new(0.05, 0, 0, 130)
noclipBtn.Text = "Ativar Noclip"
noclipBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
noclipBtn.TextColor3 = Color3.fromRGB(255, 0, 0)
noclipBtn.Font = Enum.Font.SourceSansBold
noclipBtn.TextScaled = true
noclipBtn.Parent = frame

local noclipAtivo = false
noclipBtn.MouseButton1Click:Connect(function()
	noclipAtivo = not noclipAtivo
	noclipBtn.Text = noclipAtivo and "Desativar Noclip" or "Ativar Noclip"
end)

RunService.Stepped:Connect(function()
	if noclipAtivo and LocalPlayer.Character then
		for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = false
			end
		end
	end
end)
