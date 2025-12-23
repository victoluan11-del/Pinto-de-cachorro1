-- ================================
-- GOMES PVP  BY @gomesznx.03💜 (SEM ESP)
-- AIMBOT RAGE GRUDANDO + NOCLIP
-- ================================

-- SERVIÇOS
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- PERSONAGEM
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local HRP = Character:WaitForChild("HumanoidRootPart")

LocalPlayer.CharacterAdded:Connect(function(char)
	Character = char
	Humanoid = char:WaitForChild("Humanoid")
	HRP = char:WaitForChild("HumanoidRootPart")
end)

-- ================================
-- VARIÁVEIS
-- ================================
local speedEnabled = false
local noclipEnabled = false
local infJumpEnabled = false
local flyEnabled = false
local aimbotEnabled = false

local flySpeed = 60
local FOV = 150

local BodyGyro, BodyVelocity

-- ================================
-- GUI
-- ================================
local Gui = Instance.new("ScreenGui", LocalPlayer.PlayerGui)
Gui.Name = "GOMES PVP BY @gomesznx.03💜"
Gui.ResetOnSpawn = false

local Main = Instance.new("Frame", Gui)
Main.Size = UDim2.new(0,420,0,560)
Main.Position = UDim2.new(0.3,0,0.2,0)
Main.BackgroundColor3 = Color3.fromRGB(30,30,30)
Main.BorderSizePixel = 0
Main.Active = true
Main.Draggable = true

local Title = Instance.new("TextButton", Main)
Title.Size = UDim2.new(1,0,0,50)
Title.Text = "GOMES PVP BY @gomesznx.03💜"
Title.BackgroundColor3 = Color3.fromRGB(0,0,0)
Title.TextColor3 = Color3.fromRGB(255,255,255)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 22
Title.BorderSizePixel = 0

local Line = Instance.new("Frame", Title)
Line.Size = UDim2.new(1,0,0,3)
Line.Position = UDim2.new(0,0,1,-3)
Line.BackgroundColor3 = Color3.fromRGB(170,0,255)
Line.BorderSizePixel = 0

local Container = Instance.new("ScrollingFrame", Main)
Container.Position = UDim2.new(0,0,0,50)
Container.Size = UDim2.new(1,0,1,-50)
Container.BackgroundTransparency = 1
Container.ScrollBarThickness = 6

local UIList = Instance.new("UIListLayout", Container)
UIList.Padding = UDim.new(0,8)

-- MINIMIZAR
local open = true
Title.MouseButton1Click:Connect(function()
	open = not open
	Container.Visible = open
	Main:TweenSize(open and UDim2.new(0,420,0,560) or UDim2.new(0,420,0,50),"Out","Quad",0.25,true)
end)

-- ================================
-- FUNÇÕES UI
-- ================================
local function newButton(txt)
	local b = Instance.new("TextButton", Container)
	b.Size = UDim2.new(0,380,0,45)
	b.BackgroundColor3 = Color3.fromRGB(85,0,127)
	b.TextColor3 = Color3.new(1,1,1)
	b.Font = Enum.Font.SourceSansBold
	b.TextSize = 18
	b.BorderSizePixel = 0
	b.Text = txt
	return b
end

local function newBox(txt)
	local t = Instance.new("TextBox", Container)
	t.Size = UDim2.new(0,380,0,40)
	t.BackgroundColor3 = Color3.fromRGB(60,0,90)
	t.TextColor3 = Color3.new(1,1,1)
	t.Font = Enum.Font.SourceSans
	t.TextSize = 16
	t.Text = txt
	return t
end

-- ================================
-- CONTROLES
-- ================================
local SpeedBox = newBox("Speed (ex: 60)")
local SpeedBtn = newButton("Speed: OFF")

local JumpBtn = newButton("Infinite Jump: OFF")

local FlyBox = newBox("Fly Up Speed (ex: 60)")
local FlyBtn = newButton("Fly Up: OFF")

local AimbotBtn = newButton("Aimbot Rage: OFF")
local FOVBox = newBox("FOV (ex: 150)")

local NoclipBtn = newButton("NoClip: OFF")

-- ================================
-- SPEED
-- ================================
SpeedBtn.MouseButton1Click:Connect(function()
	speedEnabled = not speedEnabled
	if speedEnabled then
		Humanoid.WalkSpeed = tonumber(SpeedBox.Text) or 60
		SpeedBtn.Text = "Speed: ON"
	else
		Humanoid.WalkSpeed = 16
		SpeedBtn.Text = "Speed: OFF"
	end
end)

-- ================================
-- INFINITE JUMP
-- ================================
JumpBtn.MouseButton1Click:Connect(function()
	infJumpEnabled = not infJumpEnabled
	JumpBtn.Text = infJumpEnabled and "Infinite Jump: ON" or "Infinite Jump: OFF"
end)

UIS.JumpRequest:Connect(function()
	if infJumpEnabled then
		Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
	end
end)

-- ================================
-- FLY UP
-- ================================
local function startFly()
	flyEnabled = true
	Humanoid.AutoRotate = false
	Humanoid:ChangeState(Enum.HumanoidStateType.Physics)

	BodyGyro = Instance.new("BodyGyro", HRP)
	BodyGyro.P = 1e5
	BodyGyro.MaxTorque = Vector3.new(9e9,9e9,9e9)

	BodyVelocity = Instance.new("BodyVelocity", HRP)
	BodyVelocity.MaxForce = Vector3.new(9e9,9e9,9e9)
end

local function stopFly()
	flyEnabled = false
	Humanoid.AutoRotate = true
	Humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
	if BodyGyro then BodyGyro:Destroy() end
	if BodyVelocity then BodyVelocity:Destroy() end
end

FlyBtn.MouseButton1Click:Connect(function()
	if flyEnabled then
		stopFly()
		FlyBtn.Text = "Fly Up: OFF"
	else
		flySpeed = tonumber(FlyBox.Text) or 60
		startFly()
		FlyBtn.Text = "Fly Up: ON"
	end
end)

-- ================================
-- NOCLIP
-- ================================
NoclipBtn.MouseButton1Click:Connect(function()
	noclipEnabled = not noclipEnabled
	NoclipBtn.Text = noclipEnabled and "NoClip: ON" or "NoClip: OFF"
end)

RunService.Stepped:Connect(function()
	if noclipEnabled and Character then
		for _,v in pairs(Character:GetDescendants()) do
			if v:IsA("BasePart") then v.CanCollide = false end
		end
	end
end)

-- ================================
-- AIMBOT RAGE (GRUDADO)
-- ================================
local FOVCircle = Drawing.new("Circle")
FOVCircle.Color = Color3.fromRGB(170,0,255)
FOVCircle.Thickness = 2
FOVCircle.NumSides = 100
FOVCircle.Filled = false

AimbotBtn.MouseButton1Click:Connect(function()
	aimbotEnabled = not aimbotEnabled
	AimbotBtn.Text = aimbotEnabled and "Aimbot Rage: ON" or "Aimbot Rage: OFF"
end)

local function getClosestTarget()
	local closest, dist = nil, math.huge
	for _,p in pairs(Players:GetPlayers()) do
		if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("Head") and p.Character:FindFirstChild("Humanoid") then
			if p.Character.Humanoid.Health > 0 then
				local pos, onscreen = Camera:WorldToViewportPoint(p.Character.Head.Position)
				if onscreen then
					local mag = (Vector2.new(pos.X,pos.Y) -
						Vector2.new(Camera.ViewportSize.X/2,Camera.ViewportSize.Y/2)).Magnitude
					if mag < dist and mag < FOV then
						dist = mag
						closest = p
					end
				end
			end
		end
	end
	return closest
end

-- ================================
-- LOOP
-- ================================
RunService.RenderStepped:Connect(function()
	-- Fly
	if flyEnabled and BodyVelocity then
		BodyVelocity.Velocity = Vector3.new(0, flySpeed, 0)
		BodyGyro.CFrame = Camera.CFrame
	end

	-- FOV
	FOV = tonumber(FOVBox.Text) or FOV
	FOVCircle.Position = Vector2.new(Camera.ViewportSize.X/2,Camera.ViewportSize.Y/2)
	FOVCircle.Radius = FOV
	FOVCircle.Visible = aimbotEnabled

	-- Aimbot
	if aimbotEnabled then
		local target = getClosestTarget()
		if target and target.Character and target.Character:FindFirstChild("Head") then
			Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Character.Head.Position)
		end
	end
end)-- ================================
-- TP PLAYER SEGURO (NÃO TELEPORTA EM ALIADOS)
-- ================================

-- Criar botão no container
local TPPlayerSafeBtn = Instance.new("TextButton", Container)
TPPlayerSafeBtn.Size = UDim2.new(0,380,0,45)
TPPlayerSafeBtn.BackgroundColor3 = Color3.fromRGB(85,0,127)
TPPlayerSafeBtn.TextColor3 = Color3.new(1,1,1)
TPPlayerSafeBtn.Font = Enum.Font.SourceSansBold
TPPlayerSafeBtn.TextSize = 18
TPPlayerSafeBtn.Text = "TP Player Seguro"
TPPlayerSafeBtn.BorderSizePixel = 0

-- Função TP Seguro
TPPlayerSafeBtn.MouseButton1Click:Connect(function()
    local closest = nil
    local dist = math.huge

    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character.Humanoid.Health > 0 then
            -- Aqui você verifica se não é aliado (ex: time diferente)
            local isAlly = false
            -- Substitua essa verificação pelo sistema de time do seu jogo, se tiver
            -- if p.Team == LocalPlayer.Team then isAlly = true end

            if not isAlly then
                local d = (HRP.Position - p.Character.HumanoidRootPart.Position).Magnitude
                if d < dist then
                    dist = d
                    closest = p
                end
            end
        end
    end

    if closest then
        -- Teleporta um pouco atrás do player pra não atravessar parede
        local offset = Vector3.new(0,0,3)
        HRP.CFrame = CFrame.new(closest.Character.HumanoidRootPart.Position + offset)
    end
end)--// esp box simples (funcional)

local Players = game:GetService("Players")
local lp = Players.LocalPlayer

local espEnabled = false
local espBoxes = {}

local function createBox(char)
	local hrp = char:FindFirstChild("HumanoidRootPart")
	if not hrp then return end

	local box = Instance.new("BoxHandleAdornment")
	box.Name = "esp_box"
	box.Adornee = hrp
	box.AlwaysOnTop = true
	box.ZIndex = 10
	box.Size = hrp.Size
	box.Transparency = 0.4
	box.Color3 = Color3.fromRGB(170, 0, 255) -- roxo
	box.Parent = hrp

	espBoxes[char] = box
end

local function removeBox(char)
	if espBoxes[char] then
		espBoxes[char]:Destroy()
		espBoxes[char] = nil
	end
end

local function enableESP()
	for _,plr in pairs(Players:GetPlayers()) do
		if plr ~= lp and plr.Character then
			createBox(plr.Character)
		end
	end
end

local function disableESP()
	for _,box in pairs(espBoxes) do
		box:Destroy()
	end
	espBoxes = {}
end

Players.PlayerAdded:Connect(function(plr)
	plr.CharacterAdded:Connect(function(char)
		if espEnabled then
			task.wait(1)
			createBox(char)
		end
	end)
end)

Players.PlayerRemoving:Connect(function(plr)
	if plr.Character then
		removeBox(plr.Character)
	end
end)

-- BOTÃO (usa o mesmo estilo dos seus)
local espButton = Instance.new("TextButton")
espButton.Size = UDim2.new(0,120,0,40)
espButton.Position = UDim2.new(0.05,0,0.7,0)
espButton.Text = "esp: off"
espButton.BackgroundColor3 = Color3.fromRGB(90,0,130)
espButton.TextColor3 = Color3.new(1,1,1)
espButton.Font = Enum.Font.GothamBold
espButton.TextSize = 14
espButton.Parent = game.CoreGui

Instance.new("UICorner", espButton)

espButton.MouseButton1Click:Connect(function()
	espEnabled = not espEnabled

	if espEnabled then
		espButton.Text = "esp: on"
		enableESP()
	else
		espButton.Text = "esp: off"
		disableESP()
	end
end)--// Fly OFC (Botão Roxo) - PC + Mobile
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local camera = workspace.CurrentCamera

local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local hrp = character:WaitForChild("HumanoidRootPart")

-- CONFIG
local flySpeed = 60
local flying = false

local BodyGyro, BodyVelocity
local keys = {W=false,A=false,S=false,D=false}

-- Inputs PC
UserInputService.InputBegan:Connect(function(input,gpe)
	if gpe then return end
	if input.KeyCode == Enum.KeyCode.W then keys.W=true end
	if input.KeyCode == Enum.KeyCode.A then keys.A=true end
	if input.KeyCode == Enum.KeyCode.S then keys.S=true end
	if input.KeyCode == Enum.KeyCode.D then keys.D=true end
end)

UserInputService.InputEnded:Connect(function(input)
	if input.KeyCode == Enum.KeyCode.W then keys.W=false end
	if input.KeyCode == Enum.KeyCode.A then keys.A=false end
	if input.KeyCode == Enum.KeyCode.S then keys.S=false end
	if input.KeyCode == Enum.KeyCode.D then keys.D=false end
end)

-- START / STOP FLY
local function startFly()
	flying = true
	humanoid.AutoRotate = false
	humanoid:ChangeState(Enum.HumanoidStateType.Physics)
	BodyGyro = Instance.new("BodyGyro", hrp)
	BodyGyro.P = 1e5
	BodyGyro.MaxTorque = Vector3.new(9e9,9e9,9e9)
	BodyVelocity = Instance.new("BodyVelocity", hrp)
	BodyVelocity.MaxForce = Vector3.new(9e9,9e9,9e9)
end

local function stopFly()
	flying = false
	humanoid.AutoRotate = true
	humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
	if BodyGyro then BodyGyro:Destroy() end
	if BodyVelocity then BodyVelocity:Destroy() end
end

-- LOOP FLY
RunService.RenderStepped:Connect(function()
	if not flying then return end
	local camCF = camera.CFrame
	local move = Vector3.zero
	if keys.W then move += camCF.LookVector end
	if keys.S then move -= camCF.LookVector end
	if keys.D then move += camCF.RightVector end
	if keys.A then move -= camCF.RightVector end
	local moveDir = humanoid.MoveDirection
	if moveDir.Magnitude > 0 then move += camCF.LookVector * moveDir.Magnitude end
	if move.Magnitude > 0 then move = move.Unit * flySpeed end
	BodyVelocity.Velocity = move
	BodyGyro.CFrame = CFrame.new(hrp.Position, hrp.Position + camCF.LookVector)
end)

-- BOTÃO FLY OFC
local Gui = Instance.new("ScreenGui", player.PlayerGui)
Gui.Name = "FlyOFCGui"
Gui.ResetOnSpawn = false

local Btn = Instance.new("TextButton", Gui)
Btn.Size = UDim2.new(0, 120, 0, 40)
Btn.Position = UDim2.new(0.02,0,0.2,0)
Btn.Text = "fly ofc"
Btn.BackgroundColor3 = Color3.fromRGB(170,0,255)
Btn.TextColor3 = Color3.new(1,1,1)
Btn.Font = Enum.Font.GothamBold
Btn.TextSize = 16
Instance.new("UICorner", Btn)

Btn.MouseButton1Click:Connect(function()
	if flying then
		stopFly()
		Btn.BackgroundColor3 = Color3.fromRGB(170,0,255)
	else
		startFly()
		Btn.BackgroundColor3 = Color3.fromRGB(85,0,255)
	end
end)

-- Respawn fix
player.CharacterAdded:Connect(function(char)
	character = char
	humanoid = char:WaitForChild("Humanoid")
	hrp = char:WaitForChild("HumanoidRootPart")
end)-- ================================
-- ANTI KNOCKBACK ULTRA | BOTÃO
-- ================================
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

local MAX_HORIZONTAL_SPEED = 14
local MASS_OVERRIDE = PhysicalProperties.new(100, 0.3, 0.5, 1, 1)

-- ================================
-- VARIÁVEL
-- ================================
local antiKBEnabled = false

-- ================================
-- BOTÃO NO PAINEL
-- ================================
local AntiKBBtn = Instance.new("TextButton", Container)
AntiKBBtn.Size = UDim2.new(0,380,0,45)
AntiKBBtn.BackgroundColor3 = Color3.fromRGB(150,0,0)
AntiKBBtn.TextColor3 = Color3.new(1,1,1)
AntiKBBtn.Font = Enum.Font.SourceSansBold
AntiKBBtn.TextSize = 18
AntiKBBtn.Text = "Anti KB: OFF"
AntiKBBtn.BorderSizePixel = 0

AntiKBBtn.MouseButton1Click:Connect(function()
	antiKBEnabled = not antiKBEnabled
	if antiKBEnabled then
		AntiKBBtn.Text = "Anti KB: ON"
		AntiKBBtn.BackgroundColor3 = Color3.fromRGB(0,160,0)
	else
		AntiKBBtn.Text = "Anti KB: OFF"
		AntiKBBtn.BackgroundColor3 = Color3.fromRGB(150,0,0)
	end
end)

-- ================================
-- FUNÇÃO CORE
-- ================================
local function clearForces()
	for _,v in ipairs(HumanoidRootPart:GetChildren()) do
		if v:IsA("BodyMover") or v:IsA("VectorForce") or v:IsA("LinearVelocity") then
			v:Destroy()
		end
	end
end

RunService.Stepped:Connect(function()
	if not antiKBEnabled then return end
	clearForces()
	local vel = HumanoidRootPart.AssemblyLinearVelocity
	HumanoidRootPart.AssemblyLinearVelocity = Vector3.new(
		math.clamp(vel.X, -MAX_HORIZONTAL_SPEED, MAX_HORIZONTAL_SPEED),
		vel.Y,
		math.clamp(vel.Z, -MAX_HORIZONTAL_SPEED, MAX_HORIZONTAL_SPEED)
	)
end)

RunService.Heartbeat:Connect(function()
	if not antiKBEnabled then return end
	clearForces()
end)

-- Aplica peso extremo
for _,p in ipairs(Character:GetDescendants()) do
	if p:IsA("BasePart") then
		p.CustomPhysicalProperties = MASS_OVERRIDE
	end
end-- ================================
-- ESP LINE SYSTEM | BOTÃO NO PAINEL
-- ================================
local espLineEnabled = false
local espObjects = {}

-- Remove ESP de um jogador
local function removeESP(plr)
	if espObjects[plr] then
		for _,v in pairs(espObjects[plr]) do
			if v.Remove then v:Remove() end
		end
	end
	espObjects[plr] = nil
end

-- Cria ESP para um jogador
local function createESP(plr)
	if plr == LocalPlayer then return end
	espObjects[plr] = {}

	local line = Drawing.new("Line")
	line.Thickness = 2
	line.Color = Color3.fromRGB(0,255,255) -- CIANO
	line.Visible = false

	table.insert(espObjects[plr], line)

	RunService.RenderStepped:Connect(function()
		if not plr.Character or not plr.Character:FindFirstChild("HumanoidRootPart") then
			line.Visible = false
			return
		end

		local hrp = plr.Character.HumanoidRootPart
		local pos, onScreen = Camera:WorldToViewportPoint(hrp.Position)

		if espLineEnabled and onScreen then
			line.From = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y)
			line.To = Vector2.new(pos.X, pos.Y)
			line.Visible = true
		else
			line.Visible = false
		end
	end)
end

-- Inicializa ESP para todos os jogadores existentes
for _,p in pairs(Players:GetPlayers()) do
	createESP(p)
end

-- Detecta novos jogadores
Players.PlayerAdded:Connect(createESP)
Players.PlayerRemoving:Connect(removeESP)

-- ================================
-- BOTÃO NO PAINEL
-- ================================
local ESPLineBtn = newButton("ESP LINE: OFF")

ESPLineBtn.MouseButton1Click:Connect(function()
	espLineEnabled = not espLineEnabled
	if espLineEnabled then
		ESPLineBtn.Text = "ESP LINE: ON"
	else
		ESPLineBtn.Text = "ESP LINE: OFF"
	end
end)-- ================================
-- BOTÃO EMOTE GUI (VISUAL)
-- ================================
local emoteButton = Instance.new("TextButton")
emoteButton.Size = UDim2.new(0,380,0,45)
emoteButton.BackgroundColor3 = Color3.fromRGB(85,0,127)
emoteButton.TextColor3 = Color3.new(1,1,1)
emoteButton.Font = Enum.Font.SourceSansBold
emoteButton.TextSize = 18
emoteButton.Text = "emote gui (visual)"
emoteButton.BorderSizePixel = 0
emoteButton.Parent = Container  -- garante que fica dentro do painel principal

emoteButton.MouseButton1Click:Connect(function()
    loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Emote-Gui-75782"))()
end)--==================== FLY 2 (LOADSTRING) ====================

local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- evita executar mais de uma vez
if getgenv().Fly2Loaded then return end
getgenv().Fly2Loaded = true

-- GUI
local gui = Instance.new("ScreenGui")
gui.Name = "Fly2Gui"
gui.ResetOnSpawn = false
gui.Parent = game.CoreGui

local button = Instance.new("TextButton", gui)
button.Size = UDim2.new(0, 130, 0, 45)
button.Position = UDim2.new(0.02, 0, 0.28, 0) -- 🔽 praticamente grudado embaixo do outro fly
button.BackgroundColor3 = Color3.fromRGB(140, 0, 255) -- roxo
button.Text = "fly 2"
button.TextColor3 = Color3.new(1,1,1)
button.Font = Enum.Font.GothamBold
button.TextSize = 16
button.Active = true
button.Draggable = true
Instance.new("UICorner", button).CornerRadius = UDim.new(0,12)

local loaded = false

button.MouseButton1Click:Connect(function()
	if loaded then return end
	loaded = true
	button.Text = "fly 2: on"

	pcall(function()
		loadstring(game:HttpGet(
			"https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"
		))()
	end)
end)

--============================================================--==============================
-- GRAVITY CONTROL (BUTTON + SLIDER)
--==============================

local GravityEnabled = false
local GravityValue = workspace.Gravity

-- BOTÃO GRAVIDADE
local GravityBtn = Instance.new("TextButton", Container)
GravityBtn.Size = UDim2.new(0.9, 0, 0, 40)
GravityBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 130)
GravityBtn.Te
