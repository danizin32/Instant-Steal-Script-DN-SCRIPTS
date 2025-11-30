--========================================================
-- CARREGAMENTO RGB PROFISSIONAL - DN SCRIPTS
--========================================================

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local TweenService = game:GetService("TweenService")

-- Tela de carregamento
local LoadGui = Instance.new("ScreenGui", LocalPlayer.PlayerGui)
LoadGui.IgnoreGuiInset = true
LoadGui.ResetOnSpawn = false

local LoadFrame = Instance.new("Frame", LoadGui)
LoadFrame.Size = UDim2.new(1,0,1,0)
LoadFrame.BackgroundColor3 = Color3.new(0,0,0)

local Title = Instance.new("TextLabel", LoadFrame)
Title.Size = UDim2.new(1,0,0.15,0)
Title.Position = UDim2.new(0,0,0.4,0)
Title.BackgroundTransparency = 1
Title.Text = "DN SCRIPTS"
Title.TextColor3 = Color3.fromRGB(255,255,255)
Title.Font = Enum.Font.GothamBlack
Title.TextScaled = true

local Bar = Instance.new("Frame", LoadFrame)
Bar.Size = UDim2.new(0.6,0,0.03,0)
Bar.Position = UDim2.new(0.2,0,0.58,0)
Bar.BackgroundColor3 = Color3.fromRGB(30,30,30)
Bar.BorderSizePixel = 0
Bar.ClipsDescendants = true
Bar.BackgroundTransparency = 0.3

local Fill = Instance.new("Frame", Bar)
Fill.Size = UDim2.new(0,0,1,0)
Fill.Position = UDim2.new(0,0,0,0)
Fill.BackgroundColor3 = Color3.fromRGB(255,0,0)
Fill.BorderSizePixel = 0

-- Animação RGB na barra
task.spawn(function()
	while true do
		for i = 0, 1, 0.01 do
			Fill.BackgroundColor3 = Color3.fromHSV(i,1,1)
			task.wait(0.02)
		end
	end
end)

-- Animação enchendo a barra
TweenService:Create(Fill, TweenInfo.new(2, Enum.EasingStyle.Linear), {Size = UDim2.new(1,0,1,0)}):Play()
task.wait(2.5)
LoadGui:Destroy()

--========================================================
-- GUI PRINCIPAL AJUSTADA PARA MOBILE
--========================================================

local bornPosition = nil

local ScreenGui = Instance.new("ScreenGui", LocalPlayer.PlayerGui)
ScreenGui.ResetOnSpawn = false

-- Interface MENOR para mobile
local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0, 240, 0, 140)  -- DIMINUÍDO
Main.Position = UDim2.new(0.5, -120, 0.5, -70)
Main.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Main.BorderSizePixel = 0
Main.BackgroundTransparency = 0.15
Main.ClipsDescendants = true

local UIC = Instance.new("UICorner", Main)
UIC.CornerRadius = UDim.new(0, 20)

local Border = Instance.new("UIStroke", Main)
Border.Thickness = 3

-- Efeito RGB na borda
task.spawn(function()
	while true do
		for i = 0, 1, 0.01 do
			Border.Color = Color3.fromHSV(i, 1, 1)
			task.wait(0.02)
		end
	end
end)

-- Título
local Title2 = Instance.new("TextLabel", Main)
Title2.Size = UDim2.new(1, 0, 0.25, 0)
Title2.BackgroundTransparency = 1
Title2.Text = "MozilOnTop"
Title2.TextColor3 = Color3.fromRGB(255,255,255)
Title2.Font = Enum.Font.GothamBold
Title2.TextScaled = true

-- Botão menor para mobile
local Btn = Instance.new("TextButton", Main)
Btn.Size = UDim2.new(0.85, 0, 0.28, 0) -- reduzido
Btn.Position = UDim2.new(0.075, 0, 0.35, 0)
Btn.Text = "Instant Steal"
Btn.Font = Enum.Font.GothamBold
Btn.TextColor3 = Color3.fromRGB(255,255,200)
Btn.TextScaled = true
Btn.BorderSizePixel = 0
Btn.AutoButtonColor = false

local BtnCorner = Instance.new("UICorner", Btn)
BtnCorner.CornerRadius = UDim.new(0, 18)

-- Gradiente igual ao da imagem
local gradient = Instance.new("UIGradient", Btn)
gradient.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0, Color3.fromRGB(120, 0, 120)),
	ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255, 255, 0)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 140, 0))
})

-- READY reduzido
local Status = Instance.new("TextLabel", Main)
Status.Size = UDim2.new(1,0,0.2,0)  -- menor
Status.Position = UDim2.new(0,0,0.67,0)
Status.BackgroundTransparency = 1
Status.Text = "Ready"
Status.TextColor3 = Color3.fromRGB(200,200,200)
Status.Font = Enum.Font.Gotham
Status.TextScaled = true

--========================================================
-- ARRASTAR A INTERFACE (MOBILE + PC)
--========================================================

local dragging = false
local dragStart, startPos

Main.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = Main.Position
	end
end)

Main.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = false
	end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
	if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
		local delta = input.Position - dragStart
		Main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

--========================================================
-- SALVAR POSIÇÃO DE NASCIMENTO
--========================================================

repeat task.wait() until LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
bornPosition = LocalPlayer.Character.HumanoidRootPart.CFrame

--========================================================
-- FUNÇÃO DO BOTÃO
--========================================================

Btn.MouseButton1Click:Connect(function()
	if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
		LocalPlayer.Character.HumanoidRootPart.CFrame = bornPosition
	end
end)
