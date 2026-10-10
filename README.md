--========================================================--
--        JABJOUBA AIM ASSIST - MOBILE + PC
--        Roblox Studio - للعبة الخاصة بك
--========================================================--

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local Player = Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

--========================================================--
-- SETTINGS
--========================================================--

local Settings = {
	Enabled = false,

	FOV = 180,
	Smoothness = 0.18,
	MaxDistance = 500,

	AimPart = "Head",

	TeamCheck = true,
	WallCheck = true
}

local Target = nil

--========================================================--
-- SCREEN GUI
--========================================================--

local Gui = Instance.new("ScreenGui")
Gui.Name = "JabjoubaAimAssist"
Gui.ResetOnSpawn = false
Gui.IgnoreGuiInset = true
Gui.Parent = PlayerGui

--========================================================--
-- OPEN BUTTON
--========================================================--

local OpenButton = Instance.new("TextButton")
OpenButton.Name = "OpenButton"
OpenButton.Size = UDim2.fromOffset(170, 52)
OpenButton.Position = UDim2.new(0, 20, 0.5, -26)
OpenButton.BackgroundColor3 = Color3.fromRGB(25,25,35)
OpenButton.BorderSizePixel = 0
OpenButton.Text = "🎯 AIM MENU"
OpenButton.TextColor3 = Color3.fromRGB(255,255,255)
OpenButton.Font = Enum.Font.GothamBold
OpenButton.TextSize = 15
OpenButton.Parent = Gui

local OpenCorner = Instance.new("UICorner")
OpenCorner.CornerRadius = UDim.new(0,12)
OpenCorner.Parent = OpenButton

local OpenStroke = Instance.new("UIStroke")
OpenStroke.Color = Color3.fromRGB(80,120,255)
OpenStroke.Thickness = 2
OpenStroke.Parent = OpenButton

--========================================================--
-- MAIN
--========================================================--

local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.fromOffset(390,460)
Main.Position = UDim2.new(0.5,-195,0.5,-230)
Main.BackgroundColor3 = Color3.fromRGB(18,18,25)
Main.BorderSizePixel = 0
Main.Visible = false
Main.ClipsDescendants = false
Main.Parent = Gui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0,16)
MainCorner.Parent = Main

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = Color3.fromRGB(80,120,255)
MainStroke.Thickness = 2
MainStroke.Parent = Main

--========================================================--
-- TOP BAR
--========================================================--

local Top = Instance.new("Frame")
Top.Name = "Top"
Top.Size = UDim2.new(1,0,0,60)
Top.BackgroundColor3 = Color3.fromRGB(25,25,35)
Top.BorderSizePixel = 0
Top.Parent = Main

local TopCorner = Instance.new("UICorner")
TopCorner.CornerRadius = UDim.new(0,16)
TopCorner.Parent = Top

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1,-65,1,0)
Title.Position = UDim2.fromOffset(15,0)
Title.BackgroundTransparency = 1
Title.Text = "🎯  JABJOUBA AIM ASSIST"
Title.TextColor3 = Color3.fromRGB(255,255,255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 17
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Top

local Close = Instance.new("TextButton")
Close.Size = UDim2.fromOffset(40,40)
Close.Position = UDim2.new(1,-50,0,10)
Close.BackgroundColor3 = Color3.fromRGB(220,55,65)
Close.BorderSizePixel = 0
Close.Text = "×"
Close.TextColor3 = Color3.new(1,1,1)
Close.Font = Enum.Font.GothamBold
Close.TextSize = 25
Close.Parent = Top

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0,10)
CloseCorner.Parent = Close

--========================================================--
-- CONTENT
--========================================================--

local Content = Instance.new("ScrollingFrame")
Content.Name = "Content"
Content.Size = UDim2.new(1,-20,1,-75)
Content.Position = UDim2.fromOffset(10,68)
Content.BackgroundTransparency = 1
Content.BorderSizePixel = 0
Content.ScrollBarThickness = 5
Content.CanvasSize = UDim2.new(0,0,0,650)
Content.AutomaticCanvasSize = Enum.AutomaticSize.Y
Content.Parent = Main

local Layout = Instance.new("UIListLayout")
Layout.Padding = UDim.new(0,9)
Layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
Layout.Parent = Content

--========================================================--
-- GUI HELPERS
--========================================================--

local function Label(text)

	local L = Instance.new("TextLabel")

	L.Size = UDim2.new(1,-20,0,28)
	L.BackgroundTransparency = 1
	L.Text = text
	L.TextColor3 = Color3.fromRGB(220,220,230)
	L.Font = Enum.Font.GothamBold
	L.TextSize = 13
	L.TextXAlignment = Enum.TextXAlignment.Left
	L.Parent = Content

	return L
end

local function Button(text)

	local B = Instance.new("TextButton")

	B.Size = UDim2.new(1,-20,0,48)
	B.BackgroundColor3 = Color3.fromRGB(45,45,60)
	B.BorderSizePixel = 0
	B.Text = text
	B.TextColor3 = Color3.new(1,1,1)
	B.Font = Enum.Font.GothamBold
	B.TextSize = 14
	B.Parent = Content

	local C = Instance.new("UICorner")
	C.CornerRadius = UDim.new(0,10)
	C.Parent = B

	return B
end

--========================================================--
-- AIM ON/OFF
--========================================================--

local AimButton = Button("🔴 AIM ASSIST : OFF")

AimButton.MouseButton1Click:Connect(function()

	Settings.Enabled = not Settings.Enabled

	if Settings.Enabled then
		AimButton.Text = "🟢 AIM ASSIST : ON"
		AimButton.BackgroundColor3 = Color3.fromRGB(45,170,90)
	else
		AimButton.Text = "🔴 AIM ASSIST : OFF"
		AimButton.BackgroundColor3 = Color3.fromRGB(170,45,55)
		Target = nil
	end
end)

--========================================================--
-- FOV
--========================================================--

Label("🔵 FOV")

local FOVButton = Button("FOV : 180")

FOVButton.MouseButton1Click:Connect(function()

	Settings.FOV += 30

	if Settings.FOV > 420 then
		Settings.FOV = 60
	end

	FOVButton.Text = "FOV : " .. Settings.FOV
end)

--========================================================--
-- SMOOTHNESS
--========================================================--

Label("🎚️ AIM SMOOTHNESS")

local SmoothButton = Button("Smoothness : 0.18")

SmoothButton.MouseButton1Click:Connect(function()

	Settings.Smoothness += 0.05

	if Settings.Smoothness > 1 then
		Settings.Smoothness = 0.05
	end

	SmoothButton.Text =
		"Smoothness : " ..
		string.format("%.2f",Settings.Smoothness)
end)

--========================================================--
-- AIM PART
--========================================================--

Label("🎯 TARGET PART")

local PartButton = Button("Target : HEAD")

local Parts = {
	"Head",
	"UpperTorso",
	"HumanoidRootPart"
}

local PartIndex = 1

PartButton.MouseButton1Click:Connect(function()

	PartIndex += 1

	if PartIndex > #Parts then
		PartIndex = 1
	end

	Settings.AimPart = Parts[PartIndex]

	PartButton.Text =
		"Target : " ..
		string.upper(Settings.AimPart)
end)

--========================================================--
-- TEAM CHECK
--========================================================--

local TeamButton = Button("👥 TEAM CHECK : ON")

TeamButton.MouseButton1Click:Connect(function()

	Settings.TeamCheck = not Settings.TeamCheck

	TeamButton.Text =
		"👥 TEAM CHECK : " ..
		(Settings.TeamCheck and "ON" or "OFF")
end)

--========================================================--
-- WALL CHECK
--========================================================--

local WallButton = Button("🧱 WALL CHECK : ON")

WallButton.MouseButton1Click:Connect(function()

	Settings.WallCheck = not Settings.WallCheck

	WallButton.Text =
		"🧱 WALL CHECK : " ..
		(Settings.WallCheck and "ON" or "OFF")
end)

--========================================================--
-- MAX DISTANCE
--========================================================--

local DistanceButton = Button("📏 DISTANCE : 500")

DistanceButton.MouseButton1Click:Connect(function()

	Settings.MaxDistance += 250

	if Settings.MaxDistance > 1500 then
		Settings.MaxDistance = 100
	end

	DistanceButton.Text =
		"📏 DISTANCE : " ..
		Settings.MaxDistance
end)

--========================================================--
-- DRAG SYSTEM
-- يعمل بالماوس واللمس
--========================================================--

local function MakeDraggable(Object, Handle)

	local Dragging = false
	local DragStart
	local StartPosition

	Handle.InputBegan:Connect(function(Input)

		if Input.UserInputType == Enum.UserInputType.MouseButton1
			or Input.UserInputType == Enum.UserInputType.Touch then

			Dragging = true
			DragStart = Input.Position
			StartPosition = Object.Position
		end
	end)

	UserInputService.InputChanged:Connect(function(Input)

		if not Dragging then
			return
		end

		if Input.UserInputType == Enum.UserInputType.MouseMovement
			or Input.UserInputType == Enum.UserInputType.Touch then

			local Delta = Input.Position - DragStart

			Object.Position = UDim2.new(
				StartPosition.X.Scale,
				StartPosition.X.Offset + Delta.X,
				StartPosition.Y.Scale,
				StartPosition.Y.Offset + Delta.Y
			)
		end
	end)

	UserInputService.InputEnded:Connect(function(Input)

		if Input.UserInputType == Enum.UserInputType.MouseButton1
			or Input.UserInputType == Enum.UserInputType.Touch then

			Dragging = false
		end
	end)
end

-- زر الفتح يتحرك من أي مكان فيه
MakeDraggable(OpenButton,OpenButton)

-- النافذة تتحرك من الـTop Bar
MakeDraggable(Main,Top)

--========================================================--
-- OPEN / CLOSE
--========================================================--

OpenButton.MouseButton1Click:Connect(function()

	-- لا نفتح إذا كان المستخدم يحاول سحب الزر
	Main.Visible = true
	OpenButton.Visible = false
end)

Close.MouseButton1Click:Connect(function()

	Main.Visible = false
	OpenButton.Visible = true
end)

--========================================================--
-- RESIZE SYSTEM
-- 4 زوايا
--========================================================--

local MinWidth = 300
local MinHeight = 330

local function CreateResizeHandle(Name, Position, AnchorPoint)

	local Handle = Instance.new("TextButton")

	Handle.Name = Name
	Handle.Size = UDim2.fromOffset(28,28)
	Handle.Position = Position
	Handle.AnchorPoint = AnchorPoint

	Handle.BackgroundTransparency = 1
	Handle.Text = ""
	Handle.AutoButtonColor = false
	Handle.ZIndex = 20
	Handle.Parent = Main

	return Handle
end

-- أسفل يمين
local BottomRight = CreateResizeHandle(
	"ResizeBottomRight",
	UDim2.new(1,0,1,0),
	Vector2.new(1,1)
)

-- أسفل يسار
local BottomLeft = CreateResizeHandle(
	"ResizeBottomLeft",
	UDim2.new(0,0,1,0),
	Vector2.new(0,1)
)

-- أعلى يمين
local TopRight = CreateResizeHandle(
	"ResizeTopRight",
	UDim2.new(1,0,0,0),
	Vector2.new(1,0)
)

-- أعلى يسار
local TopLeft = CreateResizeHandle(
	"ResizeTopLeft",
	UDim2.new(0,0,0,0),
	Vector2.new(0,0)
)

local function Resize(Object,Handle,CornerType)

	local Resizing = false
	local StartMouse
	local StartSize
	local StartPosition

	Handle.InputBegan:Connect(function(Input)

		if Input.UserInputType == Enum.UserInputType.MouseButton1
			or Input.UserInputType == Enum.UserInputType.Touch then

			Resizing = true
			StartMouse = Input.Position
			StartSize = Object.AbsoluteSize
			StartPosition = Object.AbsolutePosition
		end
	end)

	UserInputService.InputChanged:Connect(function(Input)

		if not Resizing then
			return
		end

		if Input.UserInputType ~= Enum.UserInputType.MouseMovement
			and Input.UserInputType ~= Enum.UserInputType.Touch then
			return
		end

		local Delta = Input.Position - StartMouse

		local Width = StartSize.X
		local Height = StartSize.Y

		local PosX = StartPosition.X
		local PosY = StartPosition.Y

		if CornerType == "BR" then

			Width = StartSize.X + Delta.X
			Height = StartSize.Y + Delta.Y

		elseif CornerType == "BL" then

			Width = StartSize.X - Delta.X
			Height = StartSize.Y + Delta.Y

			PosX = StartPosition.X + Delta.X

		elseif CornerType == "TR" then

			Width = StartSize.X + Delta.X
			Height = StartSize.Y - Delta.Y

			PosY = StartPosition.Y + Delta.Y

		elseif CornerType == "TL" then

			Width = StartSize.X - Delta.X
			Height = StartSize.Y - Delta.Y

			PosX = StartPosition.X + Delta.X
			PosY = StartPosition.Y + Delta.Y
		end

		Width = math.max(MinWidth,Width)
		Height = math.max(MinHeight,Height)

		Object.Size = UDim2.fromOffset(
			Width,
			Height
		)

		Object.Position = UDim2.fromOffset(
			PosX,
			PosY
		)
	end)

	UserInputService.InputEnded:Connect(function(Input)

		if Input.UserInputType == Enum.UserInputType.MouseButton1
			or Input.UserInputType == Enum.UserInputType.Touch then

			Resizing = false
		end
	end)
end

Resize(Main,BottomRight,"BR")
Resize(Main,BottomLeft,"BL")
Resize(Main,TopRight,"TR")
Resize(Main,TopLeft,"TL")

--========================================================--
-- AIM SYSTEM
--========================================================--

local function IsEnemy(OtherPlayer)

	if not Settings.TeamCheck then
		return true
	end

	return OtherPlayer.Team ~= Player.Team
end

local function IsVisible(Part)

	if not Settings.WallCheck then
		return true
	end

	local Origin = Camera.CFrame.Position
	local Direction = Part.Position - Origin

	local Params = RaycastParams.new()

	Params.FilterType = Enum.RaycastFilterType.Exclude

	Params.FilterDescendantsInstances = {
		Player.Character
	}

	local Result = workspace:Raycast(
		Origin,
		Direction,
		Params
	)

	return Result
		and Result.Instance:IsDescendantOf(Part.Parent)
end

local function GetClosestTarget()

	local Center = Vector2.new(
		Camera.ViewportSize.X / 2,
		Camera.ViewportSize.Y / 2
	)

	local Closest = nil
	local ClosestDistance = Settings.FOV

	for _,OtherPlayer in ipairs(Players:GetPlayers()) do

		if OtherPlayer ~= Player
			and IsEnemy(OtherPlayer) then

			local Character = OtherPlayer.Character

			if Character then

				local Humanoid =
					Character:FindFirstChildOfClass("Humanoid")

				local Part =
					Character:FindFirstChild(Settings.AimPart)

				if Humanoid
					and Humanoid.Health > 0
					and Part then

					local Distance3D =
						(Camera.CFrame.Position - Part.Position).Magnitude

					if Distance3D <= Settings.MaxDistance then

						local ScreenPosition,OnScreen =
							Camera:WorldToViewportPoint(
								Part.Position
							)

						if OnScreen then

							local Distance2D =
								(
									Vector2.new(
										ScreenPosition.X,
										ScreenPosition.Y
									)
									- Center
								).Magnitude

							if Distance2D < ClosestDistance
								and IsVisible(Part) then

								ClosestDistance = Distance2D
								Closest = Part
							end
						end
					end
				end
			end
		end
	end

	return Closest
end

--========================================================--
-- AIM LOOP
--========================================================--

RunService.RenderStepped:Connect(function()

	if not Settings.Enabled then
		return
	end

	if not Target
		or not Target.Parent
		or not Target:IsDescendantOf(workspace) then

		Target = GetClosestTarget()
	end

	if Target then

		local Humanoid =
			Target.Parent:FindFirstChildOfClass("Humanoid")

		if not Humanoid
			or Humanoid.Health <= 0 then

			Target = nil
			return
		end

		if Settings.WallCheck
			and not IsVisible(Target) then

			Target = GetClosestTarget()
			return
		end

		local CameraPosition =
			Camera.CFrame.Position

		local LookAt =
			CFrame.lookAt(
				CameraPosition,
				Target.Position
			)

		Camera.CFrame =
			Camera.CFrame:Lerp(
				LookAt,
				Settings.Smoothness
			)
	end
end)

--========================================================--
-- MOBILE / PC TOGGLE
--========================================================--

UserInputService.InputBegan:Connect(function(Input,Processed)

	if Processed then
		return
	end

	if Input.KeyCode == Enum.KeyCode.Q then

		Settings.Enabled = not Settings.Enabled

		if Settings.Enabled then

			AimButton.Text =
				"🟢 AIM ASSIST : ON"

			AimButton.BackgroundColor3 =
				Color3.fromRGB(45,170,90)

		else

			AimButton.Text =
				"🔴 AIM ASSIST : OFF"

			AimButton.BackgroundColor3 =
				Color3.fromRGB(170,45,55)

			Target = nil
		end
	end
end)

--========================================================--
-- MOBILE TOUCH BUTTON
--========================================================--

local TouchToggle = Instance.new("TextButton")

TouchToggle.Name = "MobileAimButton"
TouchToggle.Size = UDim2.fromOffset(64,64)
TouchToggle.Position = UDim2.new(1,-85,1,-100)
TouchToggle.BackgroundColor3 = Color3.fromRGB(35,35,45)
TouchToggle.BorderSizePixel = 0
TouchToggle.Text = "🎯"
TouchToggle.TextSize = 27
TouchToggle.Parent = Gui

local TouchCorner = Instance.new("UICorner")
TouchCorner.CornerRadius = UDim.new(1,0)
TouchCorner.Parent = TouchToggle

local TouchStroke = Instance.new("UIStroke")
TouchStroke.Color = Color3.fromRGB(80,120,255)
TouchStroke.Thickness = 2
TouchStroke.Parent = TouchToggle

TouchToggle.MouseButton1Click:Connect(function()

	Settings.Enabled = not Settings.Enabled

	if Settings.Enabled then

		TouchToggle.BackgroundColor3 =
			Color3.fromRGB(45,170,90)

		AimButton.Text =
			"🟢 AIM ASSIST : ON"

	else

		TouchToggle.BackgroundColor3 =
			Color3.fromRGB(35,35,45)

		AimButton.Text =
			"🔴 AIM ASSIST : OFF"

		Target = nil
	end
end)
