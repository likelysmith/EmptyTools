DontShowDuplicateUI = false

if not game:IsLoaded() then
	game.Loaded:Wait()
end

-- Services
UserInputService = game:GetService("UserInputService")
TextChatService = game:GetService("TextChatService")
TeleportService = game:GetService("TeleportService")
PhysicsService = game:GetService("PhysicsService")
TweenService = game:GetService("TweenService")
HttpService = game:GetService("HttpService")
RunService = game:GetService("RunService")
Lighting = game:GetService("Lighting")
Players = game:GetService("Players")
COREGUI = game:GetService("CoreGui")

queueTeleport = (syn and syn.queue_on_teleport) or queue_on_teleport or (fluxus and fluxus.queue_on_teleport)
httpRequest = (syn and syn.request) or (http and http.request) or http_request or (fluxus and fluxus.request) or request


-- Changelog

local VersionString = "v1.3.4"
local ChangeLog = "• Extended & Expanded Platform\n• Updated VC Unban with advanced capability\n• Added cross-game persistance\n• Added a Voice Chat Unban command" 
local Info = "Empty Tools is a free and open-source command menu designed to work in every experience. For more scripts and information, go to https://empty.tools and join our discord."

-- Variables

Player = Players.LocalPlayer
Mouse = Player:GetMouse()

Camera = workspace.CurrentCamera

MainUI = nil
MarkerUI = nil

OuterFrame = nil
Contents = nil
ContentShrunken = nil
InfoFrame = nil

ToastFrame = nil
ToastText = nil
TextInc = 45
ToastQueue = {}
IsShowing = false

function GetFrameSize()
	if ToastText == nil then return end
	local b = ToastText.TextBounds
	return UDim2.new(0, b.X + TextInc, 0, b.Y + TextInc * 0.6)
end

function hasProperty(object, propertyName)
	local success, _ = pcall(function() 
		object[propertyName] = object[propertyName]
	end)
	return success
end

local function ETLOG(Details)
	print("[EMPTY TOOLS ] " .. Details )
end

CommandFrames = {}
SettingsFrames = {}
DefaultLighting = {}
AttachmentLocations = {"Back","Head","Front","Left","Right"}

CommandCategories = {"None"}
CurrentCategory = "None"

FadeTweenInfo = TweenInfo.new(
	0.25, 
	Enum.EasingStyle.Quint
)

PlusEnabled = false
KeySelected = false
KeyButton = nil
UIOpen = true

MarkersActive = false

PhaseEnabled = false
PhaseObjects = {}

TeleportIndicator = nil
TeleportEnabled = false
ReturnOnFall = false

DoubleJump = false
JumpInstances = 1

FlySpeed = 30

DefaultWalkSpeed = 16
DefaultJumpPower = 50
DefaultGravity = 196.2
DefaultFOV = 70
DefaultDestroyHeight = nil
DefaultMinZoomDistance = nil
DefaultMaxZoomDistance = nil
DefaultAnimations = {
	Idle1 = nil,
	Idle2 = nil,
	Walk = nil,
	Run = nil,
	Jump = nil,
	Climb = nil, 
	Fall = nil,
	Swim = nil,
	SwimIdle = nil
}

LogsOpen = false
Log = nil

ThirdPartyCommands = {}

-- Loadstring Variables

ThirdPartyLoadstrings = 
	{
		["Infinite Yield"] = 
		function()
			loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
		end,
		["CMD-X"] = 
		function()
			loadstring(game:HttpGet("https://raw.githubusercontent.com/CMD-X/CMD-X/master/Source",true))()
		end,
		["Sky Hub"] = 
		function()
			loadstring(game:HttpGet("https://raw.githubusercontent.com/yofriendfromschool1/Sky-Hub/main/SkyHub.txt"))()
		end,
	}

EmptyToolsLoadstrings =
	{
		["Default"] = 
		[[
		if not game:IsLoaded() then
				game.Loaded:Wait()
			end
		getgenv().EmptyTools = false
		game:GetService("Players").LocalPlayer:SetAttribute("NoChangeLogs", true)
		loadstring(game:HttpGet("https://raw.githubusercontent.com/likelysmith/EmptyTools/main/script"))()
		]],
		["Beta"] = [[
			if not game:IsLoaded() then
				game.Loaded:Wait()
			end

			getgenv().EmptyTools = false
			game:GetService("Players").LocalPlayer:SetAttribute("NoChangeLogs", true)
			game:GetService("Players").LocalPlayer:SetAttribute("EmptyToolz", true)
			loadstring(game:HttpGet("https://raw.githubusercontent.com/likelysmith/EmptyTools/main/script"))()
		]],
		["Legacy"] = 
		'loadstring(game:HttpGet("https://raw.githubusercontent.com/likelysmith/EmptyTools/main/script"))()',
		["UNC"] = 
		function()
			loadstring(game:HttpGet("https://raw.githubusercontent.com/likelysmith/EmptyTools/main/unctest"))()
		end,
		["VCBypass"] =
		function()
			loadstring(game:HttpGet("https://raw.githubusercontent.com/likelysmith/EmptyTools/main/vc"))()
		end,
	}

BetaTPLoadstrings = 
	{
		["Chatsploit"] =
		function()
			loadstring(game:HttpGet("https://pastebin.com/raw/tZJD6QyA"))()
		end,
	}

-- Settings Control

settingsFile = "ETCONFIG.json"

defaultSettings = {
	TipsEnabled = true,
	AlertsEnabled = true,
	ThirdPartiesEnabled = false,
	PointTeleportIndicatorEnabled = true,
	MinimizeOnClose = true,
	Persist = true,
	ShowChangeLogs = true,
	changelogHiddenVersion = "",
	ToggleKey = "Equals",
	Topmost = true,
	AlertOfDuplicate = true,
}

function loadSettings()
	if not RunService:IsStudio() then
		if isfile(settingsFile) then
			local content = readfile(settingsFile)
			local success, data = pcall(function() 
				return HttpService:JSONDecode(content) 
			end)
			if success and data then
				return data
			end
		end
	end
	return defaultSettings
end

function saveSettings(settings)
	if not RunService:IsStudio() then
		local json = HttpService:JSONEncode(settings)
		writefile(settingsFile, json)
	end
end


userSettings = loadSettings()

if userSettings.ToggleKey == nil then
	userSettings.ToggleKey = defaultSettings.ToggleKey
	saveSettings(userSettings)
end


TipsEnabled = userSettings.TipsEnabled
AlertsEnabled = userSettings.AlertsEnabled
ThirdPartiesEnabled = userSettings.ThirdPartiesEnabled
PointTeleportIndicatorEnabled = userSettings.PointTeleportIndicatorEnabled
MinimizeOnClose = userSettings.MinimizeOnClose
Persist = userSettings.Persist
ShowChangeLogs = userSettings.ShowChangeLogs
ToggleKey = Enum.KeyCode[userSettings.ToggleKey]
Topmost = userSettings.Topmost
AlertOfDuplicate = userSettings.AlertOfDuplicate



-- Protect UI

function ProtectUI(UI)
	if syn and syn.protect_gui then
		syn.protect_gui(UI)
	end
	ETLOG("Gui Protected - " .. UI.Name)
end

-- Create UI Method

function CreateUI()
	local EmptyTools = Instance.new("ScreenGui")
	local OuterFrame = Instance.new("Frame")
	local UICorner = Instance.new("UICorner")
	local Contents = Instance.new("Frame")
	local Title = Instance.new("TextLabel")
	local UICorner_2 = Instance.new("UICorner")
	local UIAspectRatioConstraint = Instance.new("UIAspectRatioConstraint")
	local Commands = Instance.new("ScrollingFrame")
	local UIListLayout = Instance.new("UIListLayout")
	local Search = Instance.new("TextBox")
	local UICorner_3 = Instance.new("UICorner")
	local UITextSizeConstraint = Instance.new("UITextSizeConstraint")
	local Category = Instance.new("TextButton")
	local UICorner_4 = Instance.new("UICorner")
	local UITextSizeConstraint_2 = Instance.new("UITextSizeConstraint")
	local Info = Instance.new("Frame")
	local UICorner_5 = Instance.new("UICorner")
	local Title_2 = Instance.new("TextLabel")
	local Info_2 = Instance.new("TextLabel")
	local UIAspectRatioConstraint_2 = Instance.new("UIAspectRatioConstraint")
	local Settings = Instance.new("TextButton")
	local UICorner_6 = Instance.new("UICorner")
	local UITextSizeConstraint_3 = Instance.new("UITextSizeConstraint")
	local Settings_2 = Instance.new("ScrollingFrame")
	local UIListLayout_2 = Instance.new("UIListLayout")
	local Info_3 = Instance.new("TextButton")
	local UICorner_7 = Instance.new("UICorner")
	local UIAspectRatioConstraint_3 = Instance.new("UIAspectRatioConstraint")
	local ContentShrunken = Instance.new("Frame")
	local UICorner_8 = Instance.new("UICorner")
	local UIAspectRatioConstraint_4 = Instance.new("UIAspectRatioConstraint")
	local Close = Instance.new("TextButton")
	local UICorner_9 = Instance.new("UICorner")
	local UIAspectRatioConstraint_5 = Instance.new("UIAspectRatioConstraint")
	local Toggle = Instance.new("TextButton")
	local UICorner_10 = Instance.new("UICorner")
	local UIAspectRatioConstraint_6 = Instance.new("UIAspectRatioConstraint")
	local ToastFrame = Instance.new("Frame")
	local UICorner_11 = Instance.new("UICorner")
	local Text = Instance.new("TextLabel")

	--Properties:

	EmptyTools.Name = "EmptyTools"
	EmptyTools.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	EmptyTools.ResetOnSpawn = false
	EmptyTools.IgnoreGuiInset = true


	OuterFrame.Name = "OuterFrame"
	OuterFrame.Parent = EmptyTools
	OuterFrame.BackgroundColor3 = Color3.fromRGB(36, 36, 36)
	OuterFrame.BorderColor3 = Color3.fromRGB(0, 0, 0)
	OuterFrame.BorderSizePixel = 0
	OuterFrame.Position = UDim2.new(0.65821141, 0, 0.429824561, 0)
	OuterFrame.Size = UDim2.new(0.329648048, 0, 0.492894381, 0)
	if UserInputService.TouchEnabled then 
		OuterFrame.Size = UDim2.new(0.514648048, 0, 0.551894381, 0)
		OuterFrame.Position = UDim2.new(0.588, 0, 0.429824561, 0)
	end

	UICorner.CornerRadius = UDim.new(0.0500000007, 0)
	UICorner.Parent = OuterFrame

	Contents.Name = "Contents"
	Contents.Parent = OuterFrame
	Contents.BackgroundColor3 = Color3.fromRGB(36, 36, 36)
	Contents.BackgroundTransparency = 1.000
	Contents.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Contents.BorderSizePixel = 0
	Contents.Size = UDim2.new(1, 0, 1, 0)

	Title.Name = "Title"
	Title.Parent = Contents
	Title.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Title.BackgroundTransparency = 1.000
	Title.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Title.BorderSizePixel = 0
	Title.Position = UDim2.new(0.064338237, 0, 0.034324944, 0)
	Title.Size = UDim2.new(0.63786763, 0, 0.125858128, 0)
	Title.Font = Enum.Font.Gotham
	Title.Text = "Empty Tools"
	Title.TextColor3 = Color3.fromRGB(255,255,255)
	Title.TextScaled = true
	Title.TextSize = 14.000
	Title.TextWrapped = true
	Title.TextXAlignment = Enum.TextXAlignment.Left


	if PlusEnabled then
		Title.Size = UDim2.new(0.599,0,0.126,0)
		Title.RichText = true
		Title.Text = [[Empty Tools <font size="11" color="rgb(255, 69, 32)">beta</font> ]]
	end

	UICorner_2.CornerRadius = UDim.new(0.0500000007, 0)
	UICorner_2.Parent = Contents

	UIAspectRatioConstraint.Parent = Contents
	UIAspectRatioConstraint.AspectRatio = 1.245

	Commands.Name = "Commands"
	Commands.Parent = Contents
	Commands.Active = true
	Commands.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	Commands.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Commands.BorderSizePixel = 0
	Commands.Position = UDim2.new(0.064338237, 0, 0.295194507, 0)
	Commands.Size = UDim2.new(0.880514681, 0, 0.645308912, 0)
	Commands.CanvasSize = UDim2.new(0, 0, 3.5, 0)
	Commands.ScrollBarThickness = 13


	UIListLayout.Parent = Commands
	UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
	UIListLayout.Padding = UDim.new(0.00249999994, 0)

	Search.Name = "Search"
	Search.Parent = Contents
	Search.BackgroundColor3 = Color3.fromRGB(65, 65, 65)
	Search.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Search.BorderSizePixel = 0
	Search.Position = UDim2.new(0.064338237, 0, 0.194508016, 0)
	Search.Size = UDim2.new(0.286764711, 0, 0.080091536, 0)
	Search.Font = Enum.Font.Gotham
	Search.PlaceholderColor3 = Color3.fromRGB(149, 149, 149)
	Search.PlaceholderText = "Search.."
	Search.Text = ""
	Search.TextColor3 = Color3.fromRGB(255, 255, 255)
	Search.TextScaled = true
	Search.TextSize = 25.000
	Search.TextWrapped = true

	UICorner_3.CornerRadius = UDim.new(0.550000012, 0)
	UICorner_3.Parent = Search

	UITextSizeConstraint.Parent = Search
	UITextSizeConstraint.MaxTextSize = 23

	Category.Name = "Category"
	Category.Parent = Contents
	Category.BackgroundColor3 = Color3.fromRGB(65, 65, 65)
	Category.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Category.BorderSizePixel = 0
	Category.Position = UDim2.new(0.370332718, 0, 0.192686558, 0)
	Category.Size = UDim2.new(0.274057955, 0, 0.0800000057, 0)
	Category.Font = Enum.Font.Gotham
	Category.Text = "Filter: None"
	Category.TextColor3 = Color3.fromRGB(255, 255, 255)
	Category.TextScaled = true
	Category.TextSize = 20.000
	Category.TextWrapped = true

	UICorner_4.CornerRadius = UDim.new(0.550000012, 0)
	UICorner_4.Parent = Category

	UITextSizeConstraint_2.Parent = Category
	UITextSizeConstraint_2.MaxTextSize = 20

	Info.Name = "Info"
	Info.Parent = Contents
	Info.BackgroundColor3 = Color3.fromRGB(36, 36, 36)
	Info.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Info.BorderSizePixel = 0
	Info.Position = UDim2.new(0.00554660289, 0, -0.230156511, 0)
	Info.Size = UDim2.new(0.985446513, 0, 0.214045554, 0)
	Info.Visible = false

	UICorner_5.CornerRadius = UDim.new(0.25, 0)
	UICorner_5.Parent = Info

	Title_2.Name = "Title"
	Title_2.Parent = Info
	Title_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Title_2.BackgroundTransparency = 1.000
	Title_2.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Title_2.BorderSizePixel = 0
	Title_2.Position = UDim2.new(0.317073166, 0, 0.0645161271, 0)
	Title_2.Size = UDim2.new(0.375234514, 0, 0.290322572, 0)
	Title_2.Font = Enum.Font.GothamBold
	Title_2.Text = "Title"
	Title_2.TextColor3 = Color3.fromRGB(255, 255, 255)
	Title_2.TextScaled = true
	Title_2.TextSize = 14.000
	Title_2.TextWrapped = true

	Info_2.Name = "Info"
	Info_2.Parent = Info
	Info_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Info_2.BackgroundTransparency = 1.000
	Info_2.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Info_2.BorderSizePixel = 0
	Info_2.Position = UDim2.new(0.0450282581, 0, 0.354838699, 0)
	Info_2.Size = UDim2.new(0.915654898, 0, 0.54838711, 0)
	Info_2.Font = Enum.Font.Gotham
	Info_2.Text = "Info"
	Info_2.TextColor3 = Color3.fromRGB(255, 255, 255)
	Info_2.TextScaled = true
	Info_2.TextSize = 14.000
	Info_2.TextWrapped = true

	UIAspectRatioConstraint_2.Parent = Info
	UIAspectRatioConstraint_2.AspectRatio = 5.731

	Settings.Name = "Settings"
	Settings.Parent = Contents
	Settings.BackgroundColor3 = Color3.fromRGB(65, 65, 65)
	Settings.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Settings.BorderSizePixel = 0
	Settings.Position = UDim2.new(0.663670063, 0, 0.192686558, 0)
	Settings.Size = UDim2.new(0.244357735, 0, 0.0800000057, 0)
	Settings.Font = Enum.Font.Gotham
	Settings.Text = "Settings"
	Settings.TextColor3 = Color3.fromRGB(255, 255, 255)
	Settings.TextScaled = true
	Settings.TextSize = 20.000
	Settings.TextWrapped = true

	UICorner_6.CornerRadius = UDim.new(0.550000012, 0)
	UICorner_6.Parent = Settings

	UITextSizeConstraint_3.Parent = Settings
	UITextSizeConstraint_3.MaxTextSize = 20

	Settings_2.Name = "SettingsFrame"
	Settings_2.Parent = Contents
	Settings_2.Active = true
	Settings_2.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	Settings_2.BackgroundTransparency = 1
	Settings_2.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Settings_2.BorderSizePixel = 0
	Settings_2.Position = UDim2.new(0.064338237, 0, 0.295194507, 0)
	Settings_2.Size = UDim2.new(0.880514681, 0, 0.645308912, 0)
	Settings_2.Visible = false
	Settings_2.CanvasSize = UDim2.new(0, 0, 3.5, 0)
	Settings_2.ScrollBarThickness = 13

	UIListLayout_2.Parent = Settings_2
	UIListLayout_2.SortOrder = Enum.SortOrder.LayoutOrder
	UIListLayout_2.Padding = UDim.new(0.00249999994, 0)

	Info_3.Name = "Info"
	Info_3.Parent = OuterFrame
	Info_3.BackgroundColor3 = Color3.fromRGB(65, 65, 65)
	Info_3.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Info_3.BorderSizePixel = 0
	Info_3.Position = UDim2.new(0.701061368, 0, 0.0343248919, 0)
	Info_3.Size = UDim2.new(0.0790441632, 0, 0.125858128, 0)
	Info_3.ZIndex = 4
	Info_3.Font = Enum.Font.Roboto
	Info_3.Text = "i"
	Info_3.TextColor3 = Color3.fromRGB(255, 255, 255)
	Info_3.TextScaled = true
	Info_3.TextSize = 14.000
	Info_3.TextWrapped = true

	UICorner_7.CornerRadius = UDim.new(0.0500000007, 0)
	UICorner_7.Parent = Info_3

	UIAspectRatioConstraint_3.Parent = Info_3
	UIAspectRatioConstraint_3.AspectRatio = 1.004

	ContentShrunken.Name = "ContentShrunken"
	ContentShrunken.Parent = OuterFrame
	ContentShrunken.BackgroundColor3 = Color3.fromRGB(36, 36, 36)
	ContentShrunken.BorderColor3 = Color3.fromRGB(0, 0, 0)
	ContentShrunken.BorderSizePixel = 0
	ContentShrunken.Position = UDim2.new(0.670155883, 0, 0.0024564031, 0)
	ContentShrunken.Size = UDim2.new(0.334465146, 0, 0.160182983, 0)
	ContentShrunken.Visible = false

	UICorner_8.CornerRadius = UDim.new(0.0500000007, 0)
	UICorner_8.Parent = ContentShrunken

	UIAspectRatioConstraint_4.Parent = OuterFrame
	UIAspectRatioConstraint_4.AspectRatio = 1.245

	Close.Name = "Close"
	Close.Parent = OuterFrame
	Close.BackgroundColor3 = Color3.fromRGB(84, 34, 34)
	Close.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Close.BorderSizePixel = 0
	Close.Position = UDim2.new(0.893432915, 0, 0.0343249068, 0)
	Close.Size = UDim2.new(0.0790441632, 0, 0.125858128, 0)
	Close.ZIndex = 4
	Close.Font = Enum.Font.Gotham
	Close.Text = "X"
	Close.TextColor3 = Color3.fromRGB(255, 255, 255)
	Close.TextScaled = true
	Close.TextSize = 14.000
	Close.TextWrapped = true

	UICorner_9.CornerRadius = UDim.new(0.0500000007, 0)
	UICorner_9.Parent = Close

	UIAspectRatioConstraint_5.Parent = Close
	UIAspectRatioConstraint_5.AspectRatio = 1.004

	Toggle.Name = "Toggle"
	Toggle.Parent = OuterFrame
	Toggle.BackgroundColor3 = Color3.fromRGB(65, 65, 65)
	Toggle.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Toggle.BorderSizePixel = 0
	Toggle.Position = UDim2.new(0.798575521, 0, 0.0343248919, 0)
	Toggle.Size = UDim2.new(0.0790441632, 0, 0.125858128, 0)
	Toggle.ZIndex = 4
	Toggle.Font = Enum.Font.Roboto
	Toggle.Text = "-"
	Toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
	Toggle.TextScaled = true
	Toggle.TextSize = 14.000
	Toggle.TextWrapped = true

	UICorner_10.CornerRadius = UDim.new(0.0500000007, 0)
	UICorner_10.Parent = Toggle

	UIAspectRatioConstraint_6.Parent = Toggle
	UIAspectRatioConstraint_6.AspectRatio = 1.004

	ToastFrame.Name = "ToastFrame"
	ToastFrame.Parent = EmptyTools
	ToastFrame.AnchorPoint = Vector2.new(0.5, 0.5)
	ToastFrame.BackgroundColor3 = Color3.fromRGB(36, 36, 36)
	ToastFrame.BorderColor3 = Color3.fromRGB(0, 0, 0)
	ToastFrame.BorderSizePixel = 0
	ToastFrame.Position = UDim2.new(0.499656826, 0, 0.0427135676, 0)
	ToastFrame.Size = UDim2.new(0.427590936, 0, 0.0527638197, 0)
	ToastFrame.Visible = false
	ToastFrame.ZIndex = 99

	UICorner_11.CornerRadius = UDim.new(0.400000006, 0)
	UICorner_11.Parent = ToastFrame

	Text.Name = "ToastText"
	Text.Parent = EmptyTools
	Text.AnchorPoint = Vector2.new(0.5, 0.5)
	Text.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Text.BackgroundTransparency = 1.000
	Text.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Text.BorderSizePixel = 0
	Text.Position = UDim2.new(0.499313653, 0, 0.0427135676, 0)
	Text.Size = UDim2.new(0.0179999992, 0, 0.0529999994, 0)
	Text.Visible = false
	Text.Font = Enum.Font.Gotham
	Text.Text = "[nil]"
	Text.TextColor3 = Color3.fromRGB(255, 255, 255)
	Text.TextSize = 24.000
	Text.ZIndex = 100

	return EmptyTools
end

function CreateAlert(Title_Text, Body_Text, Image_ID, Button_Text, Sub_Button_Text, TextAlignment, Callback, SubCallback)
	local AlertScreen = Instance.new("ScreenGui")
	local Overlay = Instance.new("TextButton")
	local CallDialog = Instance.new("ImageButton")
	local AlertContents = Instance.new("ImageLabel")
	local Footer = Instance.new("ImageLabel")
	local layout = Instance.new("UIListLayout")
	local margin = Instance.new("UIPadding")
	local Buttons = Instance.new("ImageLabel")
	local _1 = Instance.new("ImageButton")
	local ButtonContent = Instance.new("Frame")
	local ButtonMiddleContent = Instance.new("Frame")
	local UIListLayout = Instance.new("UIListLayout")
	local Text = Instance.new("TextLabel")
	local layout_2 = Instance.new("UIListLayout")
	local margin_2 = Instance.new("UIPadding")
	local layout_3 = Instance.new("UIListLayout")
	local MiddleContent = Instance.new("ImageLabel")
	local layout_4 = Instance.new("UIListLayout")
	local Image = Instance.new("ImageLabel")
	local layout_5 = Instance.new("UIListLayout")
	local margin_3 = Instance.new("UIPadding")
	local Image_2 = Instance.new("ImageLabel")
	local margin_4 = Instance.new("UIPadding")
	local Content = Instance.new("ImageLabel")
	local BodyText = Instance.new("TextLabel")
	local layout_6 = Instance.new("UIListLayout")
	local margin_5 = Instance.new("UIPadding")
	local TitleContainer = Instance.new("ImageLabel")
	local TitleArea = Instance.new("ImageLabel")
	local layout_7 = Instance.new("UIListLayout")
	local Title = Instance.new("TextLabel")
	local Underline = Instance.new("Frame")
	local margin_6 = Instance.new("UIPadding")
	local layout_8 = Instance.new("UIListLayout")
	local margin_7 = Instance.new("UIPadding")
	local margin_8 = Instance.new("UIPadding")

	if not Button_Text then
		Button_Text = "Ok"
	end
	if not Title_Text then
		Title_Text = "Alert"
	end
	local Parent
	
	if RunService:IsStudio() then
		Parent = Player.PlayerGui
	else 
		Parent = COREGUI.RobloxGui
		ProtectUI(AlertScreen)
	end
	

	AlertScreen.Name = "AlertScreen"
	AlertScreen.Parent = Parent
	AlertScreen.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	AlertScreen.IgnoreGuiInset = true
	AlertScreen.DisplayOrder = 150


	Overlay.Name = "Overlay"
	Overlay.Parent = AlertScreen
	Overlay.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
	Overlay.BackgroundTransparency = 0.500
	Overlay.BorderSizePixel = 0
	Overlay.Selectable = false
	Overlay.Size = UDim2.new(1, 0, 1, 0)
	Overlay.Visible = false
	Overlay.AutoButtonColor = false
	Overlay.Text = ""

	CallDialog.Name = "CallDialog"
	CallDialog.Parent = AlertScreen
	CallDialog.AnchorPoint = Vector2.new(0.5, 0.5)
	CallDialog.BackgroundColor3 = Color3.fromRGB(57, 59, 61)
	CallDialog.BackgroundTransparency = 1.000
	CallDialog.BorderSizePixel = 0
	CallDialog.ClipsDescendants = true
	CallDialog.Position = UDim2.new(0.5, 0, 0.5, 0)
	CallDialog.Selectable = false
	CallDialog.Size = UDim2.new(0, 400, 0, 182)
	CallDialog.AutoButtonColor = false
	CallDialog.Image = "rbxasset://LuaPackages/Packages/_Index/FoundationImages/FoundationImages/SpriteSheets/img_set_1x_1.png"
	CallDialog.ImageColor3 = Color3.fromRGB(57, 59, 61)
	CallDialog.ImageRectOffset = Vector2.new(402, 494)
	CallDialog.ImageRectSize = Vector2.new(17, 17)
	CallDialog.ScaleType = Enum.ScaleType.Slice
	CallDialog.SliceCenter = Rect.new(8, 8, 9, 9)
	CallDialog.AutomaticSize = Enum.AutomaticSize.Y

	AlertContents.Name = "AlertContents"
	AlertContents.Parent = CallDialog
	AlertContents.BackgroundTransparency = 1.000
	AlertContents.BorderSizePixel = 0
	AlertContents.Size = UDim2.new(0, 400, 0, 182)

	Footer.Name = "Footer"
	Footer.Parent = AlertContents
	Footer.AutomaticSize = Enum.AutomaticSize.Y
	Footer.BackgroundTransparency = 1.000
	Footer.LayoutOrder = 3
	Footer.Size = UDim2.new(1, 0, 0, 36)

	layout.Name = "$layout"
	layout.Parent = Footer
	layout.SortOrder = Enum.SortOrder.LayoutOrder
	layout.Padding = UDim.new(0, 12)

	margin.Name = "$margin"
	margin.Parent = Footer

	Buttons.Name = "Buttons"
	Buttons.Parent = Footer
	Buttons.AutomaticSize = Enum.AutomaticSize.Y
	Buttons.BackgroundTransparency = 1.000
	Buttons.LayoutOrder = 3
	Buttons.Size = UDim2.new(1, 0, 0, 36)

	_1.Name = "1"
	_1.Parent = Buttons
	_1.BackgroundTransparency = 1.000
	_1.LayoutOrder = 1
	_1.AutomaticSize = Enum.AutomaticSize.Y
	_1.Size = UDim2.new(0, 352, 0, 36)
	_1.AutoButtonColor = false
	_1.Image = "rbxasset://LuaPackages/Packages/_Index/FoundationImages/FoundationImages/SpriteSheets/img_set_1x_1.png"
	_1.ImageRectOffset = Vector2.new(402, 494)
	_1.ImageRectSize = Vector2.new(17, 17)
	_1.ScaleType = Enum.ScaleType.Slice
	_1.SliceCenter = Rect.new(8, 8, 9, 9)

	ButtonContent.Name = "ButtonContent"
	ButtonContent.Parent = _1
	ButtonContent.BackgroundTransparency = 1.000
	ButtonContent.Size = UDim2.new(1, 0, 1, 0)

	ButtonMiddleContent.Name = "ButtonMiddleContent"
	ButtonMiddleContent.Parent = ButtonContent
	ButtonMiddleContent.BackgroundTransparency = 1.000
	ButtonMiddleContent.Size = UDim2.new(1, 0, 1, 0)

	UIListLayout.Parent = ButtonMiddleContent
	UIListLayout.FillDirection = Enum.FillDirection.Horizontal
	UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
	UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
	UIListLayout.VerticalAlignment = Enum.VerticalAlignment.Center
	UIListLayout.Padding = UDim.new(0, 5)

	Text.Name = "Text"
	Text.Parent = ButtonMiddleContent
	Text.BackgroundTransparency = 1.000
	Text.LayoutOrder = 2
	Text.Size = UDim2.new(0, 23, 0, 22)
	Text.Font = Enum.Font.BuilderSansBold
	Text.Text = Button_Text
	Text.AutomaticSize = Enum.AutomaticSize.X
	Text.TextColor3 = Color3.fromRGB(57, 59, 61)
	Text.TextSize = 20.000
	Text.TextWrapped = true

	layout_2.Name = "$layout"
	layout_2.Parent = Buttons
	layout_2.FillDirection = Enum.FillDirection.Horizontal
	layout_2.HorizontalAlignment = Enum.HorizontalAlignment.Center
	layout_2.SortOrder = Enum.SortOrder.LayoutOrder

	margin_2.Name = "$margin"
	margin_2.Parent = Buttons

	layout_3.Name = "$layout"
	layout_3.Parent = AlertContents
	layout_3.SortOrder = Enum.SortOrder.LayoutOrder
	layout_3.Padding = UDim.new(0, 24)

	MiddleContent.Name = "MiddleContent"
	MiddleContent.Parent = AlertContents
	MiddleContent.AutomaticSize = Enum.AutomaticSize.Y
	MiddleContent.BackgroundTransparency = 1.000
	MiddleContent.LayoutOrder = 2
	MiddleContent.Size = UDim2.new(1, 0, 0, 22)

	layout_4.Name = "$layout"
	layout_4.Parent = MiddleContent
	layout_4.SortOrder = Enum.SortOrder.LayoutOrder

	Image.Name = "Image"
	Image.Parent = MiddleContent
	Image.BackgroundTransparency = 1.000
	Image.LayoutOrder = 2
	Image.AutomaticSize = Enum.AutomaticSize.Y
	Image.Size = UDim2.new(1, 0, 0, 22)
	Image.Visible = false


	layout_5.Name = "$layout"
	layout_5.Parent = Image
	layout_5.SortOrder = Enum.SortOrder.LayoutOrder
	layout_5.Padding = UDim.new(0, 12)

	margin_3.Name = "$margin"
	margin_3.Parent = Image
	margin_3.PaddingBottom = UDim.new(0, 15)
	margin_3.PaddingLeft = UDim.new(0, 75)

	margin_4.Name = "$margin"
	margin_4.Parent = MiddleContent

	Content.Name = "Content"
	Content.Parent = MiddleContent
	Content.BackgroundTransparency = 1.000
	Content.AutomaticSize = Enum.AutomaticSize.Y
	Content.LayoutOrder = 2
	Content.Size = UDim2.new(1, 0, 0, 22)

	BodyText.Name = "BodyText"
	BodyText.Parent = Content
	BodyText.TextXAlignment = TextAlignment
	BodyText.BackgroundTransparency = 1.000
	BodyText.LayoutOrder = 1
	BodyText.Size = UDim2.new(1, 0, 0, 22)
	BodyText.AutomaticSize = Enum.AutomaticSize.Y
	BodyText.Font = Enum.Font.BuilderSans
	BodyText.Text = Body_Text
	BodyText.TextColor3 = Color3.fromRGB(189, 190, 190)
	BodyText.TextSize = 20.000
	BodyText.TextWrapped = true

	layout_6.Name = "$layout"
	layout_6.Parent = Content
	layout_6.SortOrder = Enum.SortOrder.LayoutOrder
	layout_6.Padding = UDim.new(0, 12)

	margin_5.Name = "$margin"
	margin_5.Parent = Content

	TitleContainer.Name = "TitleContainer"
	TitleContainer.Parent = AlertContents
	TitleContainer.BackgroundTransparency = 1.000
	TitleContainer.LayoutOrder = 1
	TitleContainer.Size = UDim2.new(1, 0, 0, 52)

	TitleArea.Name = "TitleArea"
	TitleArea.Parent = TitleContainer
	TitleArea.BackgroundTransparency = 1.000
	TitleArea.LayoutOrder = 1
	TitleArea.Size = UDim2.new(1, 0, 0, 40)

	layout_7.Name = "$layout"
	layout_7.Parent = TitleArea
	layout_7.HorizontalAlignment = Enum.HorizontalAlignment.Center
	layout_7.SortOrder = Enum.SortOrder.LayoutOrder
	layout_7.Padding = UDim.new(0, 12)

	Title.Name = "Title"
	Title.Parent = TitleArea
	Title.BackgroundTransparency = 1.000
	Title.LayoutOrder = 1
	Title.AutomaticSize = Enum.AutomaticSize.X
	Title.Size = UDim2.new(0, 14, 0, 27)
	Title.Font = Enum.Font.BuilderSansBold
	Title.Text = Title_Text
	Title.TextColor3 = Color3.fromRGB(255, 255, 255)
	Title.TextSize = 25.000
	Title.TextWrapped = true

	Underline.Name = "Underline"
	Underline.Parent = TitleArea
	Underline.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Underline.BackgroundTransparency = 0.800
	Underline.BorderSizePixel = 0
	Underline.LayoutOrder = 2
	Underline.Size = UDim2.new(1, 0, 0, 1)

	margin_6.Name = "$margin"
	margin_6.Parent = TitleArea

	layout_8.Name = "$layout"
	layout_8.Parent = TitleContainer
	layout_8.HorizontalAlignment = Enum.HorizontalAlignment.Center
	layout_8.SortOrder = Enum.SortOrder.LayoutOrder
	layout_8.Padding = UDim.new(0, 8)

	margin_7.Name = "$margin"
	margin_7.Parent = TitleContainer
	margin_7.PaddingTop = UDim.new(0, 12)

	margin_8.Name = "$margin"
	margin_8.Parent = AlertContents
	margin_8.PaddingBottom = UDim.new(0, 24)
	margin_8.PaddingLeft = UDim.new(0, 24)
	margin_8.PaddingRight = UDim.new(0, 24)

	if Image_ID then
		Image.Visible = true
		Image_2.Name = "Image"
		Image_2.Parent = Image
		Image_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
		Image_2.BorderColor3 = Color3.fromRGB(0, 0, 0)
		Image_2.BorderSizePixel = 0
		Image_2.BackgroundTransparency = 1
		Image_2.Size = UDim2.new(0, 200, 0, 200)
		Image_2.Image = Image_ID
		Image_2.Visible = true
	end
	_1.MouseButton1Click:Connect(function()
		AlertScreen:Destroy()
		if Callback then
			Callback()
		end
	end)

	if Sub_Button_Text then
		local SubButton = Buttons:Clone()
		SubButton.Parent = Buttons.Parent
		local SubSubButton = SubButton:FindFirstChild("1")
		local SubSubBtnClone = SubSubButton:Clone()
		SubSubBtnClone:ClearAllChildren()
		SubSubBtnClone.Parent = SubSubButton
		local UICorner = Instance.new("UICorner", SubSubBtnClone)
		local Stroke = Instance.new("UIStroke", SubSubBtnClone)
		SubSubBtnClone.ImageTransparency = 1
		Stroke.Color = Color3.fromRGB(139, 140, 143)
		Stroke.Thickness = 1.5
		SubSubButton.ImageColor3 = Color3.fromRGB(57, 59, 61)
		SubSubButton.ButtonContent.ButtonMiddleContent.Text.Text = Sub_Button_Text
		SubSubButton.ButtonContent.ButtonMiddleContent.Text.TextColor3 = Color3.fromRGB(182, 182, 182)

		local function Hover()
			SubSubButton.MouseEnter:Connect(function()
				SubSubButton.ImageColor3 = Color3.fromRGB(43, 46, 47)
			end)
			SubSubButton.MouseLeave:Connect(function()
				SubSubButton.ImageColor3 = Color3.fromRGB(57, 59, 61)
			end)
		end

		coroutine.wrap(Hover)()

		SubSubBtnClone.MouseButton1Click:Connect(function()
			AlertScreen:Destroy()
			if SubCallback then
				SubCallback()
			end
		end)
	end

	local function Hover()
		_1.MouseEnter:Connect(function()
			_1.ImageColor3 = Color3.fromRGB(172, 172, 172)
		end)
		_1.MouseLeave:Connect(function()
			_1.ImageColor3 = Color3.fromRGB(255,255,255)
		end)
	end

	coroutine.wrap(Hover)()
	
	--for i,v in pairs(AlertScreen:GetDescendants()) do
	--	if hasProperty(v, "ZIndex") then
	--		v.ZIndex = 100
	--	end
	--end

	return AlertScreen
end

local interruptCurrentToast = false

local function ShowToast(msg, t)
	ToastText.Text = msg
	ToastText.Size = UDim2.new(0, ToastText.TextBounds.X, 0, ToastText.TextBounds.Y)
	local fs = GetFrameSize()
	local isize = UDim2.new(0, fs.X.Offset * 0.15, 0, fs.Y.Offset)
	ToastFrame.Size = isize
	ToastText.TextTransparency = 1
	ToastFrame.BackgroundTransparency = 1
	ToastText.Visible = true
	ToastFrame.Visible = true
	local sizeTween = TweenService:Create(ToastFrame, TweenInfo.new(0.5, Enum.EasingStyle.Back), {Size = fs})
	local fadeInText = TweenService:Create(ToastText, TweenInfo.new(0.55), {TextTransparency = 0})
	local fadeInFrame = TweenService:Create(ToastFrame, TweenInfo.new(0.5), {BackgroundTransparency = 0})
	sizeTween:Play()
	fadeInFrame:Play()
	wait(0.1)
	fadeInText:Play()
	fadeInText.Completed:Wait()


	local elapsed = 0
	local interval = 0.1 
	while elapsed < t do
		if interruptCurrentToast then
			break
		end
		wait(interval)
		elapsed = elapsed + interval
	end


	interruptCurrentToast = false


	local sizeTweenOut = TweenService:Create(ToastFrame, TweenInfo.new(0.6, Enum.EasingStyle.Back), {Size = isize})
	local fadeOutText = TweenService:Create(ToastText, TweenInfo.new(0.5), {TextTransparency = 1})
	local fadeOutFrame = TweenService:Create(ToastFrame, TweenInfo.new(0.5), {BackgroundTransparency = 1})
	fadeOutText:Play()
	wait(0.15)
	fadeOutFrame:Play()
	wait(0.1)
	sizeTweenOut:Play()
	fadeOutText.Completed:Wait()
	ToastText.Visible = false
	ToastText.Text = "[nil]"
	ToastFrame.Visible = false
end

local function ToastQueueRunner()
	if IsShowing then return end
	IsShowing = true
	while #ToastQueue > 0 do
		local current = table.remove(ToastQueue, 1)
		ShowToast(current.msg, current.t)
	end
	IsShowing = false
end

local function Toast(msg, t)
	if not t then t = 2 end
	if not AlertsEnabled then return end
	if not MainUI or not ToastFrame or not ToastText then return end
	for i, toast in ipairs(ToastQueue) do
		if toast.msg == msg then
			return
		end
	end
	if ToastText.Text == msg then
		return
	end
	if IsShowing then
		interruptCurrentToast = true
	end
	table.insert(ToastQueue, {msg = msg, t = t})
	ToastQueueRunner()
end




FadeOutTween = nil
FadeOutTween2 = nil
FadeOutTween3 = nil

local function TipView(LabelObject, Title, Text)
	local function FadeIn()
		if not TipsEnabled then
			InfoFrame.Visible = false
			return
		end
		InfoFrame.Visible = true
		InfoFrame.BackgroundTransparency = 1
		InfoFrame.Title.TextTransparency = 1
		InfoFrame.Info.TextTransparency = 1
		InfoFrame.Title.Text = Title
		InfoFrame.Info.Text = Text

		TweenService:Create(InfoFrame, FadeTweenInfo, {BackgroundTransparency = 0}):Play()
		for _, Label in pairs(InfoFrame:GetChildren()) do
			if Label:IsA("TextLabel") then
				TweenService:Create(Label, FadeTweenInfo, {TextTransparency = 0}):Play()
			end
		end
	end

	local function FadeOut()
		TweenService:Create(InfoFrame, FadeTweenInfo, {BackgroundTransparency = 1}):Play()
		TweenService:Create(InfoFrame.Title, FadeTweenInfo, {TextTransparency = 1}):Play()
		TweenService:Create(InfoFrame.Info, FadeTweenInfo, {TextTransparency = 1}):Play()
	end

	LabelObject.MouseEnter:Connect(FadeIn)
	LabelObject.MouseLeave:Connect(FadeOut)
end

function CreateCommand(TitleText, Info, InputText, EnumTable, FilterTag, CallbackRun, CallbackStop)

	local Command = Instance.new("Frame")
	local Title = Instance.new("TextLabel")
	local Input = Instance.new("TextBox")
	local UICorner = Instance.new("UICorner")
	local Run = Instance.new("ImageButton")
	local UICorner_2 = Instance.new("UICorner")
	local UIAspectRatioConstraint = Instance.new("UIAspectRatioConstraint")
	local Stop = Instance.new("ImageButton")
	local UICorner_3 = Instance.new("UICorner")
	local UIAspectRatioConstraint_2 = Instance.new("UIAspectRatioConstraint")
	local EnumButton = Instance.new("TextButton")
	local UITextSizeConstraint = Instance.new("UITextSizeConstraint")
	local UICorner_4 = Instance.new("UICorner")

	if not table.find(CommandCategories, FilterTag) then
		table.insert(CommandCategories, FilterTag)
	end
	Command:SetAttribute("Category", FilterTag)

	local isThirdParty = false
	if InputText == "THIRDPARTY" then
		isThirdParty = true
		InputText = nil
	end

	local SelectedEnum
	if EnumTable ~= nil then
		SelectedEnum = EnumTable[1]
	end

	Command.Name = TitleText
	Command.BackgroundColor3 = Color3.fromRGB(85, 85, 85)
	Command.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Command.BorderSizePixel = 0
	Command.Position = UDim2.new(0, 0, 1.09406805e-07, 0)
	Command.Size = UDim2.new(1, 0, 0.0219999999, 0)

	Title.Name = "Title"
	Title.Parent = Command
	Title.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Title.BackgroundTransparency = 1.000
	Title.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Title.BorderSizePixel = 0
	Title.Position = UDim2.new(0.0238096155, 0, 0.0882355049, 0)
	Title.Size = UDim2.new(0.323765904, 0, 0.823529363, 0)
	Title.Font = Enum.Font.Gotham
	Title.Text = TitleText
	Title.TextColor3 = Color3.fromRGB(255, 255, 255)
	Title.TextScaled = true
	Title.TextSize = 14.000
	Title.TextWrapped = true
	Title.TextXAlignment = Enum.TextXAlignment.Left


	TipView(Title, TitleText, Info)
	
	Input.Position = UDim2.new(0.549000025, 0, 0.074000001, 0)
	if InputText ~= nil then
		Input.Name = "Input"
		Input.Parent = Command
		Input.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
		Input.BorderColor3 = Color3.fromRGB(0, 0, 0)
		Input.BorderSizePixel = 0
		Input.Size = UDim2.new(0.263412803, 0, 0.805496335, 0)
		Input.ClearTextOnFocus = false
		Input.Font = Enum.Font.Gotham
		Input.PlaceholderColor3 = Color3.fromRGB(126, 126, 126)
		Input.PlaceholderText = InputText
		Input.Text = ""
		Input.TextColor3 = Color3.fromRGB(255, 255, 255)
		Input.TextScaled = true
		Input.TextSize = 25.000
		Input.TextWrapped = true
	end

	UICorner.CornerRadius = UDim.new(0.25, 0)
	UICorner.Parent = Input

	Run.Name = "Run"
	Run.Parent = Command
	Run.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	Run.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Run.BorderSizePixel = 0
	Run.Position = UDim2.new(0.907579541, 0, 0.0597809143, 0)
	Run.Size = UDim2.new(0.0637670681, 0, 0.850548923, 0)
	Run.Image = "http://www.roblox.com/asset/?id=18912253064"
	Run.ImageColor3 = Color3.fromRGB(106, 189, 102)
	Run.MouseButton1Click:Connect(function()
		--PlaySoundFromID(ClickSoundID, {Volume = 0.4})
		if InputText ~= nil then
			if SelectedEnum then
				CallbackRun(Input.Text, SelectedEnum)
			else
				CallbackRun(Input.Text)
			end
		elseif SelectedEnum then
			CallbackRun(SelectedEnum)
		else
			CallbackRun()
		end
	end)

	if isThirdParty or (EnumTable == nil and InputText == nil) then
		Title.Size = UDim2.new(0.623765904, 0, 0.823529363, 0)
	end

	if isThirdParty then
		Command.BackgroundColor3 = Color3.fromRGB(79, 55, 55)
		Run.ImageColor3 = Color3.fromRGB(255, 129, 129)
		Title.Text = Title.Text .. " [Third-Party]"
	end

	UICorner_2.CornerRadius = UDim.new(0.189999998, 0)
	UICorner_2.Parent = Run

	UIAspectRatioConstraint.Parent = Run

	if CallbackStop ~= nil then
		Stop.Name = "Stop"
		Stop.Parent = Command
		Stop.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
		Stop.BorderColor3 = Color3.fromRGB(0, 0, 0)
		Stop.BorderSizePixel = 0
		Stop.Position = UDim2.new(0.829146743, 0, 0.0597800016, 0)
		Stop.Size = UDim2.new(0.0637670681, 0, 0.880439401, 0)
		Stop.Image = "http://www.roblox.com/asset/?id=18912262996"
		Stop.ImageColor3 = Color3.fromRGB(180, 92, 92)
		Stop.MouseButton1Click:Connect(function()
			--PlaySoundFromID(ClickSoundID, {Volume = 0.4})
			CallbackStop()
		end)
	end

	UICorner_3.CornerRadius = UDim.new(0.189999998, 0)
	UICorner_3.Parent = Stop

	UIAspectRatioConstraint_2.Parent = Stop

	if EnumTable ~= nil then
		EnumButton.Name = "Enum"
		EnumButton.Parent = Command
		EnumButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
		EnumButton.BorderColor3 = Color3.fromRGB(0, 0, 0)
		EnumButton.BorderSizePixel = 0
		EnumButton.Position = UDim2.new(0.271358907, 0, 0.0963262469, 0)
		EnumButton.Size = UDim2.new(0.263000011, 0, 0.805000007, 0)
		EnumButton.Font = Enum.Font.Gotham
		EnumButton.Text = EnumTable[1]
		EnumButton.TextColor3 = Color3.fromRGB(255, 255, 255)
		EnumButton.TextScaled = true
		EnumButton.TextSize = 25.000
		EnumButton.TextWrapped = true
		
		if InputText == nil then
			EnumButton.Position = Input.Position
		end


		EnumButton.MouseButton1Click:Connect(function()
			local CurrentIndex = table.find(EnumTable, SelectedEnum)
			if CurrentIndex == #EnumTable then
				SelectedEnum = EnumTable[1]
			else
				SelectedEnum = EnumTable[CurrentIndex + 1]
			end
			EnumButton.Text = SelectedEnum
		end)
	end


	UITextSizeConstraint.Parent = EnumButton
	UITextSizeConstraint.MaxTextSize = 25

	UICorner_4.CornerRadius = UDim.new(0.189999998, 0)
	UICorner_4.Parent = EnumButton

	table.insert(CommandFrames, Command)
	Command.Parent = Contents:WaitForChild("Commands")

	if isThirdParty then
		return Command
	end
end

function RemoveCommand(CommandFrame)
	table.remove(CommandFrames, table.find(CommandFrames, CommandFrame))
	task.wait()
	CommandFrame:Destroy()
end

function SetOptionChecked(OptionButton, Value)
	OptionButton.Text = Value and "✓" or ""
end

function SetKeyMode(KeyButton, Value)
	if Value == "Enabled" then
		KeyButton.Text = ""
		KeyButton.BackgroundColor3 = Color3.fromRGB(117, 117, 117)
	else
		KeyButton.BackgroundColor3 = Color3.fromRGB(65, 65, 65)
		KeyButton.Text = UserInputService:GetStringForKeyCode(Value)
	end
end

function CreateSettingsOption(TitleText, Info, Type, Variable, CallbackClicked)

	local SettingsOption = Instance.new("Frame")
	local Title = Instance.new("TextLabel")
	local UICorner = Instance.new("UICorner")
	local Title_2 = Instance.new("TextLabel")
	local Check = Instance.new("TextButton")
	local UICorner_2 = Instance.new("UICorner")
	local UIAspectRatioConstraint = Instance.new("UIAspectRatioConstraint")

	--Properties:

	SettingsOption.Name = "SettingsOption"
	SettingsOption.BackgroundColor3 = Color3.fromRGB(4, 4, 4)
	SettingsOption.BorderColor3 = Color3.fromRGB(0, 0, 0)
	SettingsOption.BorderSizePixel = 0
	SettingsOption.Position = UDim2.new(1.50091054e-07, 0, 2.54941881e-07, 0)
	SettingsOption.Size = UDim2.new(0.936999857, 0, 0.0461164564, 0)
	SettingsOption.Name = TitleText

	Title.Name = "Title"
	Title.Parent = SettingsOption
	Title.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Title.BackgroundTransparency = 1.000
	Title.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Title.BorderSizePixel = 0
	Title.Position = UDim2.new(0.0238095392, 0, 0.0882362947, 0)
	Title.Size = UDim2.new(0.562692106, 0, 0.350000024, 0)
	Title.Font = Enum.Font.GothamBold
	Title.Text = TitleText
	Title.TextColor3 = Color3.fromRGB(255, 255, 255)
	Title.TextScaled = true
	Title.TextSize = 14.000
	Title.TextWrapped = true
	Title.TextXAlignment = Enum.TextXAlignment.Left

	UICorner.Parent = SettingsOption

	Title_2.Name = "Info"
	Title_2.Parent = SettingsOption
	Title_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Title_2.BackgroundTransparency = 1.000
	Title_2.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Title_2.BorderSizePixel = 0
	Title_2.Position = UDim2.new(0.0240000002, 0, 0.43599999, 0)
	Title_2.Size = UDim2.new(0.657068729, 0, 0.534385622, 0)
	Title_2.Font = Enum.Font.Gotham
	Title_2.Text = Info
	Title_2.TextColor3 = Color3.fromRGB(255, 255, 255)
	Title_2.TextScaled = true
	Title_2.TextSize = 14.000
	Title_2.TextWrapped = true
	Title_2.TextXAlignment = Enum.TextXAlignment.Left

	Check.Name = "Check"
	Check.Parent = SettingsOption
	Check.BackgroundColor3 = Color3.fromRGB(65, 65, 65)
	Check.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Check.BorderSizePixel = 0
	Check.Position = UDim2.new(0.842000008, 0, 0.0930000022, 0)
	Check.Size = UDim2.new(0.123999998, 0, 0.806999981, 0)
	Check.Font = Enum.Font.SourceSans
	Check.TextColor3 = Color3.fromRGB(255, 255, 255)
	Check.TextScaled = true
	Check.Text = ""
	Check.TextSize = 14.000
	Check.TextWrapped = true

	if Type == "Boolean" then
		if Variable == true then
			Check.Text = "✓"
		else
			Check.Text = ""
		end
	elseif Type == "Key" then
		Check.Text = UserInputService:GetStringForKeyCode(Variable)
	end

	Check.MouseButton1Click:Connect(function()
		CallbackClicked(Check)
	end)

	UICorner_2.CornerRadius = UDim.new(0.189999998, 0)
	UICorner_2.Parent = Check

	UIAspectRatioConstraint.Parent = Check

	table.insert(SettingsFrames, SettingsOption)
	SettingsOption.Parent = Contents:WaitForChild("SettingsFrame")
end

local currentlyFocusedMarker = nil

local function tweenMarker(marker, targetOffset)
	--local tween = TweenService:Create(marker, TweenInfo.new(0.2, Enum.EasingStyle.Quad), {StudsOffset = targetOffset})
	--tween:Play()
end

local function tweenStroke(stroke, targetThickness)
	if stroke then
		local tween = TweenService:Create(stroke, TweenInfo.new(0.2, Enum.EasingStyle.Quad), {Thickness = targetThickness})
		tween:Play()
	end
end

local function isMouseOver(button)
	local mousePos = UserInputService:GetMouseLocation() 
	local pos = button.AbsolutePosition
	local size = button.AbsoluteSize
	return mousePos.X >= pos.X and mousePos.X <= pos.X + size.X 
		and mousePos.Y >= pos.Y and mousePos.Y <= pos.Y + size.Y
end


-- Freecam Setup (Long)

local pi    = math.pi
local abs   = math.abs
local clamp = math.clamp
local exp   = math.exp
local rad   = math.rad
local sign  = math.sign
local sqrt  = math.sqrt
local tan   = math.tan

local ContextActionService = game:GetService("ContextActionService")
local RunService = game:GetService("RunService")
local StarterGui = game:GetService("StarterGui")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local Settings = UserSettings()
local GameSettings = Settings.GameSettings

local LocalPlayer = Players.LocalPlayer
if not LocalPlayer then
	Players:GetPropertyChangedSignal("LocalPlayer"):Wait()
	LocalPlayer = Players.LocalPlayer
end

local Camera = Workspace.CurrentCamera
Workspace:GetPropertyChangedSignal("CurrentCamera"):Connect(function()
	local newCamera = Workspace.CurrentCamera
	if newCamera then
		Camera = newCamera
	end
end)

local FFlagUserExitFreecamBreaksWithShiftlock
do
	local success, result = pcall(function()
		return UserSettings():IsUserFeatureEnabled("UserExitFreecamBreaksWithShiftlock")
	end)
	FFlagUserExitFreecamBreaksWithShiftlock = success and result
end

------------------------------------------------------------------------

local INPUT_PRIORITY = Enum.ContextActionPriority.High.Value

local NAV_GAIN = Vector3.new(1, 1, 1)*64
local PAN_GAIN = Vector2.new(0.75, 1)*8
local FOV_GAIN = 300

local PITCH_LIMIT = rad(90)

local VEL_STIFFNESS = 1.5
local PAN_STIFFNESS = 1.0
local FOV_STIFFNESS = 4.0

------------------------------------------------------------------------

local Spring = {} do
	Spring.__index = Spring

	function Spring.new(freq, pos)
		local self = setmetatable({}, Spring)
		self.f = freq
		self.p = pos
		self.v = pos*0
		return self
	end

	function Spring:Update(dt, goal)
		local f = self.f*2*pi
		local p0 = self.p
		local v0 = self.v

		local offset = goal - p0
		local decay = exp(-f*dt)

		local p1 = goal + (v0*dt - offset*(f*dt + 1))*decay
		local v1 = (f*dt*(offset*f - v0) + v0)*decay

		self.p = p1
		self.v = v1

		return p1
	end

	function Spring:Reset(pos)
		self.p = pos
		self.v = pos*0
	end
end

------------------------------------------------------------------------

cameraPos = Vector3.new()
cameraRot = Vector2.new()
cameraFov = 0

velSpring = Spring.new(VEL_STIFFNESS, Vector3.new())
panSpring = Spring.new(PAN_STIFFNESS, Vector2.new())
fovSpring = Spring.new(FOV_STIFFNESS, 0)

------------------------------------------------------------------------

local Input = {} do
	local thumbstickCurve do
		local K_CURVATURE = 2.0
		local K_DEADZONE = 0.15

		local function fCurve(x)
			return (exp(K_CURVATURE*x) - 1)/(exp(K_CURVATURE) - 1)
		end

		local function fDeadzone(x)
			return fCurve((x - K_DEADZONE)/(1 - K_DEADZONE))
		end

		function thumbstickCurve(x)
			return sign(x)*clamp(fDeadzone(abs(x)), 0, 1)
		end
	end

	local gamepad = {
		ButtonX = 0,
		ButtonY = 0,
		DPadDown = 0,
		DPadUp = 0,
		ButtonL2 = 0,
		ButtonR2 = 0,
		Thumbstick1 = Vector2.new(),
		Thumbstick2 = Vector2.new(),
	}

	local keyboard = {
		W = 0,
		A = 0,
		S = 0,
		D = 0,
		E = 0,
		Q = 0,
		U = 0,
		H = 0,
		J = 0,
		K = 0,
		I = 0,
		Y = 0,
		Up = 0,
		Down = 0,
		LeftShift = 0,
		RightShift = 0,
	}

	local mouse = {
		Delta = Vector2.new(),
		MouseWheel = 0,
	}

	local NAV_GAMEPAD_SPEED  = Vector3.new(1, 1, 1)
	local NAV_KEYBOARD_SPEED = Vector3.new(1, 1, 1)
	local PAN_MOUSE_SPEED    = Vector2.new(1, 1)*(pi/64)
	local PAN_GAMEPAD_SPEED  = Vector2.new(1, 1)*(pi/8)
	local FOV_WHEEL_SPEED    = 1.0
	local FOV_GAMEPAD_SPEED  = 0.25
	local NAV_ADJ_SPEED      = 0.75
	local NAV_SHIFT_MUL      = 0.25

	local navSpeed = 1

	function Input.Vel(dt)
		navSpeed = clamp(navSpeed + dt*(keyboard.Up - keyboard.Down)*NAV_ADJ_SPEED, 0.01, 4)

		local kGamepad = Vector3.new(
			thumbstickCurve(gamepad.Thumbstick1.X),
			thumbstickCurve(gamepad.ButtonR2) - thumbstickCurve(gamepad.ButtonL2),
			thumbstickCurve(-gamepad.Thumbstick1.Y)
		)*NAV_GAMEPAD_SPEED

		local kKeyboard = Vector3.new(
			keyboard.D - keyboard.A + keyboard.K - keyboard.H,
			keyboard.E - keyboard.Q + keyboard.I - keyboard.Y,
			keyboard.S - keyboard.W + keyboard.J - keyboard.U
		)*NAV_KEYBOARD_SPEED

		local shift = UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) or UserInputService:IsKeyDown(Enum.KeyCode.RightShift)

		return (kGamepad + kKeyboard)*(navSpeed*(shift and NAV_SHIFT_MUL or 1))
	end

	function Input.Pan(dt)
		local kGamepad = Vector2.new(
			thumbstickCurve(gamepad.Thumbstick2.Y),
			thumbstickCurve(-gamepad.Thumbstick2.X)
		)*PAN_GAMEPAD_SPEED
		local kMouse = mouse.Delta*PAN_MOUSE_SPEED
		mouse.Delta = Vector2.new()
		return kGamepad + kMouse
	end

	function Input.Fov(dt)
		local kGamepad = (gamepad.ButtonX - gamepad.ButtonY)*FOV_GAMEPAD_SPEED
		local kMouse = mouse.MouseWheel*FOV_WHEEL_SPEED
		mouse.MouseWheel = 0
		return kGamepad + kMouse
	end

	do
		local function Keypress(action, state, input)
			keyboard[input.KeyCode.Name] = state == Enum.UserInputState.Begin and 1 or 0
			return Enum.ContextActionResult.Sink
		end

		local function GpButton(action, state, input)
			gamepad[input.KeyCode.Name] = state == Enum.UserInputState.Begin and 1 or 0
			return Enum.ContextActionResult.Sink
		end

		local function MousePan(action, state, input)
			local delta = input.Delta
			mouse.Delta = Vector2.new(-delta.y, -delta.x)
			return Enum.ContextActionResult.Sink
		end

		local function Thumb(action, state, input)
			gamepad[input.KeyCode.Name] = input.Position
			return Enum.ContextActionResult.Sink
		end

		local function Trigger(action, state, input)
			gamepad[input.KeyCode.Name] = input.Position.z
			return Enum.ContextActionResult.Sink
		end

		local function MouseWheel(action, state, input)
			mouse[input.UserInputType.Name] = -input.Position.z
			return Enum.ContextActionResult.Sink
		end

		local function Zero(t)
			for k, v in pairs(t) do
				t[k] = v*0
			end
		end

		function Input.StartCapture()
			ContextActionService:BindActionAtPriority("FreecamKeyboard", Keypress, false, INPUT_PRIORITY,
				Enum.KeyCode.W, Enum.KeyCode.U,
				Enum.KeyCode.A, Enum.KeyCode.H,
				Enum.KeyCode.S, Enum.KeyCode.J,
				Enum.KeyCode.D, Enum.KeyCode.K,
				Enum.KeyCode.E, Enum.KeyCode.I,
				Enum.KeyCode.Q, Enum.KeyCode.Y,
				Enum.KeyCode.Up, Enum.KeyCode.Down
			)
			ContextActionService:BindActionAtPriority("FreecamMousePan",          MousePan,   false, INPUT_PRIORITY, Enum.UserInputType.MouseMovement)
			ContextActionService:BindActionAtPriority("FreecamMouseWheel",        MouseWheel, false, INPUT_PRIORITY, Enum.UserInputType.MouseWheel)
			ContextActionService:BindActionAtPriority("FreecamGamepadButton",     GpButton,   false, INPUT_PRIORITY, Enum.KeyCode.ButtonX, Enum.KeyCode.ButtonY, Enum.KeyCode.DPadDown, Enum.KeyCode.DPadUp)
			ContextActionService:BindActionAtPriority("FreecamGamepadTrigger",    Trigger,    false, INPUT_PRIORITY, Enum.KeyCode.ButtonR2, Enum.KeyCode.ButtonL2)
			ContextActionService:BindActionAtPriority("FreecamGamepadThumbstick", Thumb,      false, INPUT_PRIORITY, Enum.KeyCode.Thumbstick1, Enum.KeyCode.Thumbstick2)
		end

		function Input.StopCapture()
			navSpeed = 1
			Zero(gamepad)
			Zero(keyboard)
			Zero(mouse)
			ContextActionService:UnbindAction("FreecamKeyboard")
			ContextActionService:UnbindAction("FreecamMousePan")
			ContextActionService:UnbindAction("FreecamMouseWheel")
			ContextActionService:UnbindAction("FreecamGamepadButton")
			ContextActionService:UnbindAction("FreecamGamepadTrigger")
			ContextActionService:UnbindAction("FreecamGamepadThumbstick")
		end
	end
end

------------------------------------------------------------------------

function GetFocusDistance(cameraFrame)
	local znear = 0.1
	local viewport = Camera.ViewportSize
	local projy = 2*tan(cameraFov/2)
	local projx = viewport.x/viewport.y*projy
	local fx = cameraFrame.rightVector
	local fy = cameraFrame.upVector
	local fz = cameraFrame.lookVector

	local minVect = Vector3.new()
	local minDist = 512

	for x = 0, 1, 0.5 do
		for y = 0, 1, 0.5 do
			local cx = (x - 0.5)*projx
			local cy = (y - 0.5)*projy
			local offset = fx*cx - fy*cy + fz
			local origin = cameraFrame.p + offset*znear
			local _, hit = Workspace:FindPartOnRay(Ray.new(origin, offset.unit*minDist))
			local dist = (hit - origin).magnitude
			if minDist > dist then
				minDist = dist
				minVect = offset.unit
			end
		end
	end

	return fz:Dot(minVect)*minDist
end

function StepFreecam(dt)
	local vel = velSpring:Update(dt, Input.Vel(dt))
	local pan = panSpring:Update(dt, Input.Pan(dt))
	local fov = fovSpring:Update(dt, Input.Fov(dt))

	local zoomFactor = sqrt(tan(rad(70/2))/tan(rad(cameraFov/2)))

	cameraFov = clamp(cameraFov + fov*FOV_GAIN*dt, 1, 120)
	cameraRot = cameraRot + pan*PAN_GAIN*(dt*zoomFactor)
	cameraRot = Vector2.new(clamp(cameraRot.x, -PITCH_LIMIT, PITCH_LIMIT), cameraRot.y)

	local cameraCFrame = CFrame.new(cameraPos)*CFrame.fromOrientation(cameraRot.x, cameraRot.y, 0)*CFrame.new(vel*NAV_GAIN*dt)
	cameraPos = cameraCFrame.p

	Camera.CFrame = cameraCFrame
	Camera.Focus = cameraCFrame*CFrame.new(0, 0, -GetFocusDistance(cameraCFrame))
	Camera.FieldOfView = cameraFov
end

------------------------------------------------------------------------

function StartFreecam()
	local cameraCFrame = Camera.CFrame
	local pitch, yaw, roll = cameraCFrame:ToEulerAnglesYXZ()

	cameraPos = cameraCFrame.p
	cameraRot = Vector2.new(pitch, yaw)
	cameraFov = Camera.FieldOfView

	velSpring:Reset(Vector3.new())
	panSpring:Reset(Vector2.new())
	fovSpring:Reset(0)

	Input.StartCapture()
	RunService:BindToRenderStep("Freecam", Enum.RenderPriority.Camera.Value, StepFreecam)
end

function StopFreecam()
	Input.StopCapture()
	RunService:UnbindFromRenderStep("Freecam")
end

-- Misc Methods

function GetCharacter(Target)
	local Character = Target.Character
	if not Character then
		Target.CharacterAdded:Wait()
		Character = Target.Character
		return Character
	end
	return Character
end

function GetRoot(Target)
	local Character = GetCharacter(Target)
	if Character then
		local Root = Character:FindFirstChild("HumanoidRootPart")
		return Root
	end
end

function GetHumanoid(Target)
	local Character = GetCharacter(Target)
	if Character then
		local Humanoid = Character:FindFirstChildOfClass("Humanoid")
		return Humanoid
	end
end

function GetPlayerFromNameSnippet(Snippet)
	if Snippet == "" then return end
	Snippet = string.lower(Snippet)
	for _,User in pairs(Players:GetPlayers()) do
		if User == Player then continue end
		local LoweredName = string.lower(User.Name)
		local LoweredDisplay = string.lower(User.DisplayName)
		if string.sub(LoweredName, 1, #Snippet) == Snippet 
			or string.sub(LoweredDisplay, 1, #Snippet) == Snippet then
			return User
		end
	end
end

function CreateTeleportIndicator()
	local Indicator = Instance.new("Part")
	Indicator.Shape = Enum.PartType.Cylinder
	Indicator.Anchored = true
	Indicator.CanCollide = true
	Indicator.Color = Color3.fromRGB(0,255,0)
	Indicator.Material = Enum.Material.Neon
	Indicator.Transparency = 0.8
	Indicator.Size = Vector3.new(0.097, 4.413, 2.379)
	Indicator.Name = "TP_Indicator"
	Indicator.Orientation = Vector3.new(0,90,90)
	local Model = Instance.new("Model", workspace)
	Indicator.Position = Model.WorldPivot.Position
	Indicator.Parent = Model
	return Model
end

function CreateMarker(TargetPlayer, Type)
	local Marker = Instance.new("BillboardGui")
	local Frame = Instance.new("Frame")
	local User = Instance.new("TextLabel")
	local UICorner = Instance.new("UICorner")
	local Display = Instance.new("TextLabel")
	local Button = Instance.new("ImageButton")
	local Stroke = Instance.new("UIStroke")

	--Properties:

	Marker.Name = TargetPlayer.Name
	Marker.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	Marker.Active = true
	Marker.Adornee = GetRoot(TargetPlayer)
	Marker.AlwaysOnTop = true
	Marker.LightInfluence = 1.000
	Marker.Size = UDim2.new(0, 150, 0, 60)

	Frame.Parent = Marker
	Frame.AnchorPoint = Vector2.new(0.5, 0.5)
	Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	Frame.BackgroundTransparency = 0.2
	Frame.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Frame.BorderSizePixel = 0
	Frame.Position = UDim2.new(0.5, 0, 0.5, 0)
	Frame.Size = UDim2.new(0.800000012, 0, 0.600000024, 0)

	User.Name = "User"
	User.Parent = Frame
	User.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	User.BackgroundTransparency = 1.000
	User.BorderColor3 = Color3.fromRGB(0, 0, 0)
	User.BorderSizePixel = 0
	User.Size = UDim2.new(1, 0, 0.5, 0)
	User.Font = Enum.Font.GothamBold
	User.Text = TargetPlayer.DisplayName
	User.TextColor3 = Color3.fromRGB(255, 255, 255)
	User.TextScaled = true
	User.TextSize = 14.000
	User.TextWrapped = true

	UICorner.Parent = Frame

	Display.Name = "Display"
	Display.Parent = Frame
	Display.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	Display.BackgroundTransparency = 1.000
	Display.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Display.BorderSizePixel = 0
	Display.Position = UDim2.new(0, 0, 0.5, 0)
	Display.Size = UDim2.new(1, 0, 0.449999988, 0)
	Display.Font = Enum.Font.Gotham
	Display.Text = "@" .. TargetPlayer.Name
	Display.TextColor3 = Color3.fromRGB(255, 255, 255)
	Display.TextScaled = true
	Display.TextSize = 14.000
	Display.TextWrapped = true

	Button.Name = "Button"
	Button.Parent = Marker
	Button.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Button.BackgroundTransparency = 1.000
	Button.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Button.BorderSizePixel = 0
	Button.Size = UDim2.new(1, 0, 1, 0)
	Button.ZIndex = 3
	Button.Image = "http://www.roblox.com/asset/?id=1415236683"

	Stroke.Name = "Select"
	Stroke.Color = Color3.fromRGB(255,255,255)
	Stroke.Thickness = 0
	Stroke.Parent = Frame
	if Player:IsFriendsWith(TargetPlayer.UserId) then
		Frame.BackgroundColor3 = Color3.fromRGB(71, 113, 177)
	end

	local defaultOffset = Marker.StudsOffset
	local hoverOffset = defaultOffset + Vector3.new(0, 5, 0)

	if Type == "Teleport" then
		Button.MouseEnter:Connect(function()
			Stroke.Thickness = 0
			TweenService:Create(Stroke, FadeTweenInfo, {Thickness = 2}):Play()
		end)

		Button.MouseLeave:Connect(function()
			TweenService:Create(Stroke, FadeTweenInfo, {Thickness = 0}):Play()
		end)
	else
		Button.Visible = false
	end

	return Marker, Button
end


function TogglePlayerCharacter(Target, Value)
	local Character = GetCharacter(Target)

	if Value then
		for _,Object in pairs(Character:GetDescendants()) do
			if not Object then continue end
			if not Object:IsA("BasePart") and not Object:IsA("Decal") then continue end
			if Object:GetAttribute("OGTransparency") then
				Object.Transparency = Object:GetAttribute("OGTransparency")
			end
			if Object.Name == "Head" then
				local Neck = Object:FindFirstChild("Neck")
				if Neck:IsA("Motor6D") then
					local OldPos = Neck:FindFirstChild("OldPos")
					if OldPos then
						Neck.C1 = OldPos.Value
						OldPos:Destroy()
					end
				end
			end
		end
	else
		for _,Object in pairs(Character:GetDescendants()) do
			if not Object then continue end
			if not Object:IsA("BasePart") and not Object:IsA("Decal") then continue end
			if Object.Transparency ~= 1 then
				Object:SetAttribute("OGTransparency", Object.Transparency)
				Object.Transparency = 1
			end
			if Object.Name == "Head" then
				local Neck = Object:FindFirstChild("Neck")
				if Neck:IsA("Motor6D") then
					local OldPos = Instance.new("CFrameValue", Neck)
					OldPos.Name = "OldPos"
					OldPos.Value = Neck.C1
					Neck.C1 += Vector3.new(0, -1935, 0)
				end
			end
		end
	end
end

function EnablePhaseForInvisibleParts()
	for _, obj in pairs(workspace:GetDescendants()) do
		if obj:IsA("BasePart") and obj.CanCollide and obj.Transparency >= 1 then
			if obj:GetAttribute("OriginalTransparency") == nil then
				obj:SetAttribute("OriginalTransparency", obj.Transparency)
			end
			obj.Transparency = 0.8
		end
	end
end

function DisablePhaseForInvisibleParts()
	for _, obj in pairs(workspace:GetDescendants()) do
		if obj:IsA("BasePart") and obj:GetAttribute("OriginalTransparency") ~= nil then
			obj.Transparency = obj:GetAttribute("OriginalTransparency")
			obj:SetAttribute("OriginalTransparency", nil)
		end
	end
end


-- Selection Control 


modeEnabled = false
originalTransparency = {} 
selectionBox = Instance.new("SelectionBox")
selectionBox.Name = "HighlightSelectionBox"
selectionBox.Color3 = Color3.new(0, 1, 0) 
selectionBox.Transparency = 0 
selectionBox.Visible = false
local currentSelection = nil
reparent = false
local reparentLocation


local function updateSelection(newPart)
	if currentSelection == newPart then
		selectionBox.Adornee = nil
		selectionBox.Visible = false
		currentSelection = nil
	else
		currentSelection = newPart
		if newPart ~= nil then
			selectionBox.Parent = newPart
		end
		selectionBox.Adornee = newPart
		selectionBox.Visible = true
	end
end

local function toWholeNum(num)
	if num == 0 then
		return 1
	end
	return math.sign(num) * math.ceil(math.abs(num) / 10)
end

local function toDecimalPoint(num, numDecimalPlaces)
	return math.floor(num * 10 ^ numDecimalPlaces) / 10 ^ numDecimalPlaces
end

local playerSpawnPoint = nil

local function SetSpawnPoint()
	local root = GetRoot(Player)
	if not root then return end
	playerSpawnPoint = root.CFrame 
	local pos = playerSpawnPoint.Position
	local x = toDecimalPoint(pos.X, 2)
	local y = toDecimalPoint(pos.Y, 2)
	local z = toDecimalPoint(pos.Z, 2)
	Toast("Spawnpoint set to (" .. x .. ", " .. y .. ", " .. z .. ")", 2)
end

local function EraseSpawnPoint()
	if playerSpawnPoint ~= nil then
		Toast("Spawnpoint removed", 1.5)
	end
	playerSpawnPoint = nil
end

carActive = false
carInputBeganConn, carInputEndedConn, carHeartbeatConn = nil
carCurrentVelocity = Vector3.new(0, 0, 0)
carAcceleration = 50
carBrakeDeceleration = 150
carHorizontalVelocity = Vector3.new(0, 0, 0)
carVerticalVelocity = 0
carKeysPressed = {}
carLastHeading = nil
carMaxSpeed = 100
carLockedY = nil

driveAnimId = "http://www.roblox.com/asset/?id=17360699557"
carAnimation = nil
driveAnimDirection = 1
driveAnimTime = 3
animSpeed = 0.5
minAnimSpeed = 0.2
maxAnimSpeed = 0.8
sittingSet = false

smoothFactor = 0.1
bodyVel, bodyGyro = nil

mobileCarInput = Vector3.new(0, 0, 0)
mobileCarBreak = false
local carUI
function createCarUI()
	carUI = Instance.new("ScreenGui")
	carUI.Parent = Players.LocalPlayer:WaitForChild("PlayerGui")

	local Frame = Instance.new("Frame")
	Frame.Parent = carUI
	Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	Frame.BorderSizePixel = 0
	Frame.Position = UDim2.new(0,20,1,-220)
	Frame.Size = UDim2.new(0,170,0,150)

	local Right = Instance.new("TextButton")
	Right.Name = "Right"
	Right.Parent = Frame
	Right.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Right.BorderSizePixel = 0
	Right.Position = UDim2.new(0.60,0,0.4,0)
	Right.Size = UDim2.new(0,40,0,40)
	Right.Font = Enum.Font.SourceSans
	Right.Text = ">"
	Right.TextColor3 = Color3.fromRGB(0, 0, 0)
	Right.TextScaled = true
	local aspectRight = Instance.new("UIAspectRatioConstraint", Right)
	aspectRight.AspectRatio = 1
	local uicornerRight = Instance.new("UICorner", Right)
	uicornerRight.CornerRadius = UDim.new(0.15,0)

	local Left = Instance.new("TextButton")
	Left.Name = "Left"
	Left.Parent = Frame
	Left.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Left.BorderSizePixel = 0
	Left.Position = UDim2.new(0.1,0,0.4,0)
	Left.Size = UDim2.new(0,40,0,40)
	Left.Font = Enum.Font.SourceSans
	Left.Text = "<"
	Left.TextColor3 = Color3.fromRGB(0, 0, 0)
	Left.TextScaled = true
	local aspectLeft = Instance.new("UIAspectRatioConstraint", Left)aspectLeft = Instance.new("UIAspectRatioConstraint", Left)
	aspectLeft.AspectRatio = 1
	local uicornerLeft = Instance.new("UICorner", Left)
	uicornerLeft.CornerRadius = UDim.new(0.15,0)

	local Fwd = Instance.new("TextButton")
	Fwd.Name = "Fwd"
	Fwd.Parent = Frame
	Fwd.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Fwd.BorderSizePixel = 0
	Fwd.Position = UDim2.new(0.35,0,0.05,0)
	Fwd.Size = UDim2.new(0,40,0,40)
	Fwd.Font = Enum.Font.SourceSans
	Fwd.Text = "∧"
	Fwd.TextColor3 = Color3.fromRGB(0, 0, 0)
	Fwd.TextScaled = true
	local aspectFwd = Instance.new("UIAspectRatioConstraint", Fwd)
	aspectFwd.AspectRatio = 1
	local uicornerFwd = Instance.new("UICorner", Fwd)
	uicornerFwd.CornerRadius = UDim.new(0.15,0)

	local Back = Instance.new("TextButton")
	Back.Name = "Back"
	Back.Parent = Frame
	Back.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Back.BorderSizePixel = 0
	Back.Position = UDim2.new(0.35,0,0.65,0)
	Back.Size = UDim2.new(0,40,0,40)
	Back.Font = Enum.Font.SourceSans
	Back.Text = "∨"
	Back.TextColor3 = Color3.fromRGB(0, 0, 0)
	Back.TextScaled = true
	local aspectBack = Instance.new("UIAspectRatioConstraint", Back)
	aspectBack.AspectRatio = 1
	local uicornerBack = Instance.new("UICorner", Back)
	uicornerBack.CornerRadius = UDim.new(0.15,0)

	local Up = Instance.new("TextButton")
	Up.Name = "Up"
	Up.Parent = Frame
	Up.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Up.BorderSizePixel = 0
	Up.Position = UDim2.new(0.95,0,0.15,0)
	Up.Size = UDim2.new(0,40,0,60)
	Up.Font = Enum.Font.SourceSans
	Up.Text = "↑"
	Up.TextColor3 = Color3.fromRGB(0, 0, 0)
	Up.TextScaled = true
	local aspectUp = Instance.new("UIAspectRatioConstraint", Up)
	aspectUp.AspectRatio = 0.66
	local uicornerUp = Instance.new("UICorner", Up)
	uicornerUp.CornerRadius = UDim.new(0.15,0)

	local Down = Instance.new("TextButton")
	Down.Name = "Down"
	Down.Parent = Frame
	Down.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Down.BorderSizePixel = 0
	Down.Position = UDim2.new(0.95,0,0.55,0)
	Down.Size = UDim2.new(0,40,0,60)
	Down.Font = Enum.Font.SourceSans
	Down.Text = "↓"
	Down.TextColor3 = Color3.fromRGB(0, 0, 0)
	Down.TextScaled = true
	local aspectDown = Instance.new("UIAspectRatioConstraint", Down)
	aspectDown.AspectRatio = 0.66
	local uicornerDown = Instance.new("UICorner", Down)
	uicornerDown.CornerRadius = UDim.new(0.15,0)
	
	local Break = Instance.new("TextButton")
	Break.Name = "Break"
	Break.Parent = Frame
	Break.BackgroundColor3 = Color3.fromRGB(255, 115, 115)
	Break.BorderSizePixel = 0
	Break.Position = UDim2.new(0.75,0,0.05,0)
	Break.Size = UDim2.new(0,40,0,50)
	Break.Font = Enum.Font.SourceSans
	Break.Text = "B"
	Break.TextColor3 = Color3.fromRGB(0, 0, 0)
	Break.TextScaled = true
	local aspectB = Instance.new("UIAspectRatioConstraint", Break)
	aspectB.AspectRatio = 0.66
	local uicornerB = Instance.new("UICorner", Break)
	uicornerB.CornerRadius = UDim.new(0.15,0)

	Fwd.MouseButton1Down:Connect(function()
		mobileCarInput = Vector3.new(mobileCarInput.X, mobileCarInput.Y, -1)
	end)
	Fwd.MouseButton1Up:Connect(function()
		mobileCarInput = Vector3.new(mobileCarInput.X, mobileCarInput.Y, 0)
	end)
	Back.MouseButton1Down:Connect(function()
		mobileCarInput = Vector3.new(mobileCarInput.X, mobileCarInput.Y, 1)
	end)
	Back.MouseButton1Up:Connect(function()
		mobileCarInput = Vector3.new(mobileCarInput.X, mobileCarInput.Y, 0)
	end)
	Left.MouseButton1Down:Connect(function()
		mobileCarInput = Vector3.new(-1, mobileCarInput.Y, mobileCarInput.Z)
	end)
	Left.MouseButton1Up:Connect(function()
		mobileCarInput = Vector3.new(0, mobileCarInput.Y, mobileCarInput.Z)
	end)
	Right.MouseButton1Down:Connect(function()
		mobileCarInput = Vector3.new(1, mobileCarInput.Y, mobileCarInput.Z)
	end)
	Right.MouseButton1Up:Connect(function()
		mobileCarInput = Vector3.new(0, mobileCarInput.Y, mobileCarInput.Z)
	end)
	Up.MouseButton1Down:Connect(function()
		mobileCarInput = Vector3.new(mobileCarInput.X, 1, mobileCarInput.Z)
	end)
	Up.MouseButton1Up:Connect(function()
		mobileCarInput = Vector3.new(mobileCarInput.X, 0, mobileCarInput.Z)
	end)
	Down.MouseButton1Down:Connect(function()
		mobileCarInput = Vector3.new(mobileCarInput.X, -1, mobileCarInput.Z)
	end)
	Down.MouseButton1Up:Connect(function()
		mobileCarInput = Vector3.new(mobileCarInput.X, 0, mobileCarInput.Z)
	end)
	
	Break.MouseButton1Down:Connect(function()
		mobileCarBreak = true
	end)
	Break.MouseButton1Up:Connect(function()
		mobileCarBreak = false
	end)
end

function destroyCarUI()
	if carUI then
		carUI:Destroy()
		carUI = nil
		mobileCarInput = Vector3.new(0,0,0)
		mobileCarBreak = false
	end
end


function StartCarMode(maxSpeedInput)
	carMaxSpeed = tonumber(maxSpeedInput) or 100
	carActive = true

	if isMobile then
		createCarUI()
	end

	local player = Players.LocalPlayer
	local UserInputService = game:GetService("UserInputService")
	local RunService = game:GetService("RunService")
	local camera = workspace.CurrentCamera
	local character = player.Character
	local hrp = character:WaitForChild("HumanoidRootPart")
	local humanoid = character:WaitForChild("Humanoid")
	humanoid.Sit = true
	carLockedY = hrp.Position.Y
	carCurrentVelocity = Vector3.new(0, 0, 0)
	carHorizontalVelocity = Vector3.new(0, 0, 0)
	carVerticalVelocity = 0
	carKeysPressed = {}
	carLastHeading = nil

	bodyVel = Instance.new("BodyVelocity")
	bodyVel.MaxForce = Vector3.new(1e5, 1e5, 1e5)
	bodyVel.Parent = hrp
	bodyVel.Velocity = Vector3.new(0, 0, 0)

	bodyGyro = Instance.new("BodyGyro")
	bodyGyro.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
	bodyGyro.Parent = hrp
	bodyGyro.CFrame = hrp.CFrame

	-- Set up key connections.
	carInputBeganConn = UserInputService.InputBegan:Connect(function(input, gameProcessed)
		if gameProcessed then return end
		if input.UserInputType == Enum.UserInputType.Keyboard then
			carKeysPressed[input.KeyCode] = true
		end
	end)
	carInputEndedConn = UserInputService.InputEnded:Connect(function(input, gameProcessed)
		if gameProcessed then return end
		if input.UserInputType == Enum.UserInputType.Keyboard then
			carKeysPressed[input.KeyCode] = false
		end
	end)


	local carAnimCycleActive = false

	carHeartbeatConn = RunService.Heartbeat:Connect(function(deltaTime)
		if not carActive then return end
		if not sittingSet then
			sittingSet = true
		end

		if not carAnimation then
			local animator = humanoid:FindFirstChildOfClass("Animator")
			if not animator then
				animator = Instance.new("Animator")
				animator.Parent = humanoid
			end
			local anim = Instance.new("Animation")
			anim.AnimationId = driveAnimId
			carAnimation = animator:LoadAnimation(anim)
			carAnimation:Play()
			carAnimation:AdjustSpeed(0)
			carAnimation.TimePosition = 3
			if not carAnimCycleActive then
				carAnimCycleActive = true
				spawn(function()
					local startTime = 3
					local endTime = 6
					local playbackSpeed = 0.4
					local cycleDuration = (endTime - startTime) / playbackSpeed
					while carActive and carAnimation do
						carAnimation:AdjustSpeed(playbackSpeed)
						wait(cycleDuration)
						carAnimation:AdjustSpeed(-playbackSpeed)
						wait(cycleDuration)
						carAnimation.TimePosition = startTime
					end
					carAnimCycleActive = false
				end)
			end
		end

		local horizontalInput = Vector3.new(0, 0, 0)
		local verticalInput = 0 

		if isMobile then
			horizontalInput = mobileCarInput
			if mobileCarBreak then
				if carHorizontalVelocity.Magnitude > 0 then
					local decelAmount = carBrakeDeceleration * deltaTime
					local newMag = carHorizontalVelocity.Magnitude - decelAmount
					if newMag < 0 then newMag = 0 end
					carHorizontalVelocity = carHorizontalVelocity.Unit * newMag
				end
				if math.abs(carVerticalVelocity) > 0 then
					local decelAmount = carBrakeDeceleration * deltaTime
					if math.abs(carVerticalVelocity) - decelAmount < 0 then
						carVerticalVelocity = 0
					else
						carVerticalVelocity = carVerticalVelocity - (decelAmount * math.sign(carVerticalVelocity))
					end
				end
			end
			verticalInput = mobileCarInput.Y
		else
			if carKeysPressed[Enum.KeyCode.W] then horizontalInput = horizontalInput + Vector3.new(0, 0, -1) end
			if carKeysPressed[Enum.KeyCode.S] then horizontalInput = horizontalInput + Vector3.new(0, 0, 1) end
			if carKeysPressed[Enum.KeyCode.A] then horizontalInput = horizontalInput + Vector3.new(-1, 0, 0) end
			if carKeysPressed[Enum.KeyCode.D] then horizontalInput = horizontalInput + Vector3.new(1, 0, 0) end

			if carKeysPressed[Enum.KeyCode.E] then verticalInput = verticalInput + 1 end
			if carKeysPressed[Enum.KeyCode.Q] then verticalInput = verticalInput - 1 end

			if carKeysPressed[Enum.KeyCode.B] then
				if carHorizontalVelocity.Magnitude > 0 then
					local decelAmount = carBrakeDeceleration * deltaTime
					local newMag = carHorizontalVelocity.Magnitude - decelAmount
					if newMag < 0 then newMag = 0 end
					carHorizontalVelocity = carHorizontalVelocity.Unit * newMag
				end
				if math.abs(carVerticalVelocity) > 0 then
					local decelAmount = carBrakeDeceleration * deltaTime
					if math.abs(carVerticalVelocity) - decelAmount < 0 then
						carVerticalVelocity = 0
					else
						carVerticalVelocity = carVerticalVelocity - (decelAmount * math.sign(carVerticalVelocity))
					end
				end
			end
		end

		if horizontalInput.Magnitude > 0 then
			horizontalInput = horizontalInput.Unit
			local camCFrame = camera.CFrame
			local camLook = Vector3.new(camCFrame.LookVector.X, 0, camCFrame.LookVector.Z).Unit
			local camRight = Vector3.new(camCFrame.RightVector.X, 0, camCFrame.RightVector.Z).Unit
			local forwardComponent = horizontalInput.Z
			local rightComponent = horizontalInput.X
			local worldHorizontal = (camLook * -forwardComponent) + (camRight * rightComponent)
			if worldHorizontal.Magnitude > 0 then
				worldHorizontal = worldHorizontal.Unit
			end
			carHorizontalVelocity = carHorizontalVelocity + (worldHorizontal * carAcceleration * deltaTime)
			if carHorizontalVelocity.Magnitude > carMaxSpeed then
				carHorizontalVelocity = carHorizontalVelocity.Unit * carMaxSpeed
			end
		end
		if verticalInput ~= 0 then
			carVerticalVelocity = carVerticalVelocity + (carAcceleration * verticalInput * deltaTime)
		else
			carVerticalVelocity = 0
		end

		carCurrentVelocity = carHorizontalVelocity + Vector3.new(0, (verticalInput ~= 0 and carVerticalVelocity or 0), 0)
		bodyVel.Velocity = bodyVel.Velocity:Lerp(carCurrentVelocity, smoothFactor)

		local flatVel = Vector3.new(carHorizontalVelocity.X, 0, carHorizontalVelocity.Z)
		if flatVel.Magnitude > 0.1 then 
			carLastHeading = flatVel.Unit 
		end

		local currentPos = hrp.Position
		if verticalInput == 0 then
			currentPos = Vector3.new(currentPos.X, carLockedY, currentPos.Z)
		else
			carLockedY = hrp.Position.Y
		end

		local desiredCFrame
		if carLastHeading then
			desiredCFrame = CFrame.new(hrp.Position, hrp.Position + carLastHeading)
		else
			desiredCFrame = hrp.CFrame
		end
		bodyGyro.CFrame = bodyGyro.CFrame:Lerp(desiredCFrame, smoothFactor)
	end)
end

local function StopCarMode()
	carActive = false
	destroyCarUI()
	sittingSet = false
	if carInputBeganConn then carInputBeganConn:Disconnect() end
	if carInputEndedConn then carInputEndedConn:Disconnect() end
	if carHeartbeatConn then carHeartbeatConn:Disconnect() end
	if carAnimation then
		carAnimation:Stop()
		carAnimation = nil
	end
	if bodyVel then
		bodyVel:Destroy()
		bodyVel = nil
	end
	if bodyGyro then
		bodyGyro:Destroy()
		bodyGyro = nil
	end
	workspace.Gravity = 196.2
end


local function TeleportToPlayer(PlayerTarget)
	local Root = GetRoot(Player)
	local TargetRoot = GetRoot(PlayerTarget)
	TargetRoot.Velocity = Vector3.zero
	if not TargetRoot then return end
	Root.CFrame = TargetRoot.CFrame
end

local function ShowChangeLogs(DSA, ClickCallback)
	if UserInputService.TouchEnabled then 
		setclipboard("https://empty.tools")
		Toast("Website copied to clipboard!", 2)
		return 
	end
	local SubText = "Don't show again"
	if not DSA then
		SubText = nil
	end
	return CreateAlert(
		"Empty Tools " .. VersionString .. " | What's New?", 
		ChangeLog,
		"http://www.roblox.com/asset/?id=18941848597", 
		"Ok", 
		SubText,
		Enum.TextXAlignment.Left,
		ClickCallback,
		function()
			ClickCallback()
			Player:SetAttribute("NoChangeLogs", true)
			userSettings.changelogHiddenVersion = VersionString
			userSettings.ShowChangeLogs = false
			saveSettings(userSettings)
		end
	)
end


local function ShowInfo(DSA, ClickCallback)
	if UserInputService.TouchEnabled then 
		if UserInputService.TouchEnabled then 
			setclipboard("https://empty.tools")
			Toast("Website copied to clipboard!", 2)
			return 
		end
		return 
	end
	local SubText = "Copy To Clipboard"
	return CreateAlert(
		"Empty Tools " .. VersionString .. " | Info", 
		Info,
		"http://www.roblox.com/asset/?id=18941848597", 
		"Ok", 
		SubText,
		Enum.TextXAlignment.Left,
		ClickCallback,
		function()
			setclipboard("https://empty.tools")
			Toast("Website copied to clipboard!", 2)
		end
	)
end
local function StopPlayerAnimation()
	local animator = GetCharacter(Player).Humanoid:FindFirstChildOfClass("Animator")
	if animator then
		for _, track in ipairs(animator:GetPlayingAnimationTracks()) do
			track:Stop()
		end
	end
end


local emoteTable = {} 
local equippedEmotes = {nil, nil, nil, nil, nil, nil, nil, nil}

local function updateEmoteSettings(Humanoid)
	local humanoidDescription = Humanoid.HumanoidDescription
	humanoidDescription:SetEmotes(emoteTable)
	humanoidDescription:SetEquippedEmotes(equippedEmotes)
end

function PlayEmote(ID, slotStr)
	local numID = tonumber(ID)
	if not numID then
		warn("Invalid emote ID")
		return
	end


	local requestedSlot = slotStr:match("Slot%s*(%d+)")
	requestedSlot = tonumber(requestedSlot)
	if not requestedSlot or requestedSlot < 1 or requestedSlot > 8 then
		warn("Invalid slot specified. Use 'Slot 1' through 'Slot 8'.")
		return
	end


	if equippedEmotes[requestedSlot] == nil then
		local openSlot = nil
		for i = 1, 8 do
			if equippedEmotes[i] == nil then
				openSlot = i
				break
			end
		end
		if not openSlot then
			warn("No available emote slots.")
			return
		end
		requestedSlot = openSlot
	end


	local Info = game:GetService("MarketplaceService"):GetProductInfo(numID)
	local key = Info.Name

	equippedEmotes[requestedSlot] = key
	emoteTable[key] = { numID }

	local Humanoid = GetHumanoid(Player)
	updateEmoteSettings(Humanoid)
end

-- Animation Control

local AnimationPacks = {
	["Ninja"] = {
		Swim = 656119721,
		SwimIdle = 656121397,
		Idle = {656117400, 656118341, 886742569},
		Jump = 656117878,
		Fall = 656115606,
		Walk = 656121766,
		Run = 656118852,
		Climb = 656114359
	},
	["Cartoony"] = {
		Swim = 742639220,
		SwimIdle = 742639812,
		Idle = {742637544, 742638445, 885477856},
		Jump = 742637942,
		Fall = 742637151,
		Walk = 742640026,
		Run = 742638842,
		Climb = 742636889
	},
	["Levitation"] = {
		Swim = 616011509,
		SwimIdle = 616012453,
		Idle = {616006778, 616008087, 886862142},
		Jump = 616008936,
		Fall = 616005863,
		Walk = 616013216,
		Run = 616010382,
		Climb = 616003713
	},
	["Stylish"] = {
		Swim = 616143378,
		SwimIdle = 616144772,
		Idle = {616136790, 616138447, 886888594},
		Jump = 616139451,
		Fall = 616134815,
		Walk = 616146177,
		Run = 616140816,
		Climb = 616133594
	},
	["Vampire"] = {
		Swim = 1083464683,
		SwimIdle = 1083467779,
		Idle = {1083445855, 1083450166, 1088037547},
		Jump = 1083455352,
		Fall = 1083443587,
		Walk = 1083473930,
		Run = 1083462077,
		Climb = 1083439238
	},
	["Robot"] = {
		Swim = 616092998,
		SwimIdle = 616094091,
		Idle = {616088211, 616089559, 885531463},
		Jump = 616090535,
		Fall = 616087089,
		Walk = 616095330,
		Run = 616091570,
		Climb = 616086039
	},
	["Toy"] = {
		Swim = 782844582,
		SwimIdle = 782845186,
		Idle = {782841498, 782845736, 980952228},
		Jump = 782847020,
		Fall = 782846423,
		Walk = 782843345,
		Run = 782842708,
		Climb = 782843869
	},
	["Bubbly"] = {
		Swim = 910028158,
		SwimIdle = 910030921,
		Idle = {910004836, 910009958, 1018536639},
		Jump = 910016857,
		Fall = 910001910,
		Walk = 910034870,
		Run = 910025107,
		Climb = 909997997
	},
	["Zombie"] = {
		Swim = 616165109,
		SwimIdle = 616166655,
		Idle = {616158929, 616160636, 885545458},
		Jump = 616161997,
		Fall = 616157476,
		Walk = 616168032,
		Run = 616163682,
		Climb = 616156119
	},
	["Elder"] = {
		Swim = 845401742,
		SwimIdle = 845403127,
		Idle = {845397899, 845400520, 901160519},
		Jump = 845398858,
		Fall = 845396048,
		Walk = 845403856,
		Run = 845386501,
		Climb = 845392038
	},
	["Superhero"] = {
		Swim = 616119360,
		SwimIdle = 616120861,
		Idle = {616111295, 616113536, 885535855},
		Jump = 616115533,
		Fall = 616108001,
		Walk = 616122287,
		Run = 616117076,
		Climb = 616104706
	},
	["Werewolf"] = {
		Swim = 1083222527,
		SwimIdle = 1083225406,
		Idle = {1083195517, 1083214717, 1099492820},
		Jump = 1083218792,
		Fall = 1083189019,
		Walk = 1083178339,
		Run = 1083216690,
		Climb = 1083182000
	},
	["Mage"] = {
		Swim = 707876443,
		SwimIdle = 707894699,
		Idle = {707742142, 707855907, 885508740},
		Jump = 707853694,
		Fall = 707829716,
		Walk = 707897309,
		Run = 707861613,
		Climb = 707826056
	},
	["Astronaut"] = {
		Swim = 891639666,
		SwimIdle = 891663592,
		Idle = {891621366, 891633237, 1047759695},
		Jump = 891627522,
		Fall = 891617961,
		Walk = 891636393,
		Run = 891636393,
		Climb = 891609353
	},
	["Pirate"] = {
		Swim = 750784579,
		SwimIdle = 750785176,
		Idle = {750781874, 750782770, 885515365},
		Jump = 750782230,
		Fall = 750780242,
		Walk = 750785693,
		Run = 750783738,
		Climb = 750779899
	},
	["Knight"] = {
		Swim = 657560551,
		SwimIdle = 657557095,
		Idle = {657595757, 657568135, 885499184},
		Jump = 658409194,
		Fall = 657600338,
		Walk = 657552124,
		Run = 657564596,
		Climb = 658360781
	},
	["Rthro"] = {
		Swim = 2510199791,
		SwimIdle = 2510201162,
		Idle = {2510197257, 2510196951, 3711062489},
		Jump = 2510197830,
		Fall = 2510195892,
		Walk = 2510202577,
		Run = 2510198475,
		Climb = 2510192778
	},
	["Oldschool"] = {
		Swim = 10921243048,
		SwimIdle = 2510201162,
		Idle = {10921230744, 10921230744, 10921230744},
		Jump = 10921242013,
		Fall = 10921241244,
		Walk = 10921244891,
		Run = 10921240218,
		Climb = 10921229866
	},
	["Bold"] = {
		Swim = 16738339158,
		SwimIdle = 16738339817,
		Idle = {16738333868, 16738334710, 16738333868},
		Jump = 10921263860,
		Fall = 16738333171,
		Walk = 16738340646,
		Run = 16738337225,
		Climb = 16738332169
	}
}



local function setAnimation(animParent, groupName, animObjName, assetId)
	local group = animParent:FindFirstChild(groupName)
	if group then
		local animObj = group:FindFirstChild(animObjName)
		if animObj then
			animObj.AnimationId = "http://www.roblox.com/asset/?id=" .. tostring(assetId)
		end
	end
end

local function SetPlayerAnimations(AnimationPack)
	local Character = GetCharacter(Player)
	local AnimationIDs = AnimationPacks[AnimationPack]

	local Animate = Character:FindFirstChild("Animate")
	if not Animate then return end
	Animate.Disabled = true
	StopPlayerAnimation()

	if AnimationPack == "None" then
		setAnimation(Animate, "idle", "Animation1", DefaultAnimations.Idle1)
		setAnimation(Animate, "idle", "Animation2", DefaultAnimations.Idle2)
		setAnimation(Animate, "walk", "WalkAnim", DefaultAnimations.Walk)
		setAnimation(Animate, "run", "RunAnim", DefaultAnimations.Run)
		setAnimation(Animate, "jump", "JumpAnim", DefaultAnimations.Jump)
		setAnimation(Animate, "climb", "ClimbAnim", DefaultAnimations.Climb)
		setAnimation(Animate, "fall", "FallAnim", DefaultAnimations.Fall)
		setAnimation(Animate, "swim", "Swim", DefaultAnimations.Swim)
		setAnimation(Animate, "swimidle", "SwimIdle", DefaultAnimations.SwimIdle)
	elseif AnimationIDs then
		setAnimation(Animate, "idle", "Animation1", AnimationIDs.Idle[1])
		setAnimation(Animate, "idle", "Animation2", AnimationIDs.Idle[2])
		setAnimation(Animate, "walk", "WalkAnim", AnimationIDs.Walk)
		setAnimation(Animate, "run", "RunAnim", AnimationIDs.Run)
		setAnimation(Animate, "jump", "JumpAnim", AnimationIDs.Jump)
		setAnimation(Animate, "climb", "ClimbAnim", AnimationIDs.Climb)
		setAnimation(Animate, "fall", "FallAnim", AnimationIDs.Fall)
		setAnimation(Animate, "swim", "Swim", AnimationIDs.Swim)
		setAnimation(Animate, "swimidle", "SwimIdle", AnimationIDs.SwimIdle)
	end
	Animate.Disabled = false
end


-- Link Control

local function CreateLinkAlert(ID)
	-- Gui to Lua
	-- Version: 3.2

	-- Instances:

	local AlertScreen = Instance.new("ScreenGui")
	local Overlay = Instance.new("TextButton")
	local CallDialog = Instance.new("ImageButton")
	local AlertContents = Instance.new("ImageLabel")
	local Footer = Instance.new("ImageLabel")
	local layout = Instance.new("UIListLayout")
	local margin = Instance.new("UIPadding")
	local Buttons = Instance.new("ImageLabel")
	local _1 = Instance.new("ImageButton")
	local ButtonContent = Instance.new("Frame")
	local ButtonMiddleContent = Instance.new("Frame")
	local UIListLayout = Instance.new("UIListLayout")
	local Text = Instance.new("TextLabel")
	local layout_2 = Instance.new("UIListLayout")
	local margin_2 = Instance.new("UIPadding")
	local layout_3 = Instance.new("UIListLayout")
	local MiddleContent = Instance.new("ImageLabel")
	local layout_4 = Instance.new("UIListLayout")
	local margin_3 = Instance.new("UIPadding")
	local Image = Instance.new("ImageLabel")
	local layout_5 = Instance.new("UIListLayout")
	local margin_4 = Instance.new("UIPadding")
	local Image_2 = Instance.new("ImageLabel")
	local Content = Instance.new("ImageLabel")
	local BodyText = Instance.new("TextLabel")
	local layout_6 = Instance.new("UIListLayout")
	local margin_5 = Instance.new("UIPadding")
	local Content_2 = Instance.new("ImageLabel")
	local layout_7 = Instance.new("UIListLayout")
	local margin_6 = Instance.new("UIPadding")
	local TextBox = Instance.new("TextBox")
	local TitleContainer = Instance.new("ImageLabel")
	local TitleArea = Instance.new("ImageLabel")
	local layout_8 = Instance.new("UIListLayout")
	local Title = Instance.new("TextLabel")
	local Underline = Instance.new("Frame")
	local margin_7 = Instance.new("UIPadding")
	local layout_9 = Instance.new("UIListLayout")
	local margin_8 = Instance.new("UIPadding")
	local margin_9 = Instance.new("UIPadding")

	--Properties:
	
	
	local Parent
	if RunService:IsStudio() then
		Parent = Player.PlayerGui
	else
		Parent = COREGUI.RobloxGui
		ProtectUI(AlertScreen)
	end
	
	
	AlertScreen.Name = "LinkScreen"
	AlertScreen.Parent = Parent
	AlertScreen.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	AlertScreen.DisplayOrder = 100

	Overlay.Name = "Overlay"
	Overlay.Parent = AlertScreen
	Overlay.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
	Overlay.BackgroundTransparency = 0.500
	Overlay.BorderSizePixel = 0
	Overlay.Selectable = false
	Overlay.Size = UDim2.new(1, 0, 1, 0)
	Overlay.Visible = false
	Overlay.AutoButtonColor = false
	Overlay.Text = ""

	CallDialog.Name = "CallDialog"
	CallDialog.Parent = AlertScreen
	CallDialog.AnchorPoint = Vector2.new(0.5, 0.5)
	CallDialog.BackgroundColor3 = Color3.fromRGB(57, 59, 61)
	CallDialog.BackgroundTransparency = 1.000
	CallDialog.BorderSizePixel = 0
	CallDialog.ClipsDescendants = true
	CallDialog.AutomaticSize = Enum.AutomaticSize.Y
	CallDialog.Position = UDim2.new(0.5, 0, 0.5, 0)
	CallDialog.Selectable = false
	CallDialog.Size = UDim2.new(0, 400, 0, 182)
	CallDialog.AutoButtonColor = false
	CallDialog.Image = "rbxasset://LuaPackages/Packages/_Index/FoundationImages/FoundationImages/SpriteSheets/img_set_1x_1.png"
	CallDialog.ImageColor3 = Color3.fromRGB(57, 59, 61)
	CallDialog.ImageRectOffset = Vector2.new(402, 494)
	CallDialog.ImageRectSize = Vector2.new(17, 17)
	CallDialog.ScaleType = Enum.ScaleType.Slice
	CallDialog.SliceCenter = Rect.new(8, 8, 9, 9)

	AlertContents.Name = "AlertContents"
	AlertContents.Parent = CallDialog
	AlertContents.BackgroundTransparency = 1.000
	AlertContents.BorderSizePixel = 0
	AlertContents.Size = UDim2.new(0, 400, 0, 182)

	Footer.Name = "Footer"
	Footer.Parent = AlertContents
	Footer.AutomaticSize = Enum.AutomaticSize.Y
	Footer.BackgroundTransparency = 1.000
	Footer.LayoutOrder = 3
	Footer.Size = UDim2.new(1, 0, 0, 36)

	layout.Name = "$layout"
	layout.Parent = Footer
	layout.SortOrder = Enum.SortOrder.LayoutOrder
	layout.Padding = UDim.new(0, 12)

	margin.Name = "$margin"
	margin.Parent = Footer

	Buttons.Name = "Buttons"
	Buttons.Parent = Footer
	Buttons.BackgroundTransparency = 1.000
	Buttons.LayoutOrder = 3
	Buttons.Size = UDim2.new(1, 0, 0, 36)

	_1.Name = "1"
	_1.Parent = Buttons
	_1.BackgroundTransparency = 1.000
	_1.LayoutOrder = 1
	_1.Size = UDim2.new(0, 352, 0, 36)
	_1.AutoButtonColor = false
	_1.Image = "rbxasset://LuaPackages/Packages/_Index/FoundationImages/FoundationImages/SpriteSheets/img_set_1x_1.png"
	_1.ImageRectOffset = Vector2.new(402, 494)
	_1.ImageRectSize = Vector2.new(17, 17)
	_1.ScaleType = Enum.ScaleType.Slice
	_1.SliceCenter = Rect.new(8, 8, 9, 9)

	ButtonContent.Name = "ButtonContent"
	ButtonContent.Parent = _1
	ButtonContent.BackgroundTransparency = 1.000
	ButtonContent.Size = UDim2.new(1, 0, 1, 0)

	ButtonMiddleContent.Name = "ButtonMiddleContent"
	ButtonMiddleContent.Parent = ButtonContent
	ButtonMiddleContent.BackgroundTransparency = 1.000
	ButtonMiddleContent.Size = UDim2.new(1, 0, 1, 0)

	UIListLayout.Parent = ButtonMiddleContent
	UIListLayout.FillDirection = Enum.FillDirection.Horizontal
	UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
	UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
	UIListLayout.VerticalAlignment = Enum.VerticalAlignment.Center
	UIListLayout.Padding = UDim.new(0, 5)

	Text.Name = "Text"
	Text.Parent = ButtonMiddleContent
	Text.BackgroundTransparency = 1.000
	Text.LayoutOrder = 2
	Text.Size = UDim2.new(0, 23, 0, 22)
	Text.Font = Enum.Font.BuilderSansBold
	Text.Text = "Done"
	Text.AutomaticSize = Enum.AutomaticSize.X
	Text.TextColor3 = Color3.fromRGB(57, 59, 61)
	Text.TextSize = 20.000
	Text.TextWrapped = true

	layout_2.Name = "$layout"
	layout_2.Parent = Buttons
	layout_2.FillDirection = Enum.FillDirection.Horizontal
	layout_2.HorizontalAlignment = Enum.HorizontalAlignment.Center
	layout_2.SortOrder = Enum.SortOrder.LayoutOrder

	margin_2.Name = "$margin"
	margin_2.Parent = Buttons

	layout_3.Name = "$layout"
	layout_3.Parent = AlertContents
	layout_3.SortOrder = Enum.SortOrder.LayoutOrder
	layout_3.Padding = UDim.new(0, 24)

	MiddleContent.Name = "MiddleContent"
	MiddleContent.Parent = AlertContents
	MiddleContent.BackgroundTransparency = 1.000
	MiddleContent.LayoutOrder = 2
	MiddleContent.AutomaticSize = Enum.AutomaticSize.Y
	MiddleContent.Size = UDim2.new(1, 0, 0, 22)

	layout_4.Name = "$layout"
	layout_4.Parent = MiddleContent
	layout_4.SortOrder = Enum.SortOrder.LayoutOrder

	margin_3.Name = "$margin"
	margin_3.Parent = MiddleContent

	Image.Name = "Image"
	Image.Parent = MiddleContent
	Image.BackgroundTransparency = 1.000
	Image.LayoutOrder = 2
	Image.Size = UDim2.new(1, 0, 0, 22)
	Image.Visible = false

	layout_5.Name = "$layout"
	layout_5.Parent = Image
	layout_5.SortOrder = Enum.SortOrder.LayoutOrder
	layout_5.Padding = UDim.new(0, 12)

	margin_4.Name = "$margin"
	margin_4.Parent = Image
	margin_4.PaddingBottom = UDim.new(0, 15)
	margin_4.PaddingLeft = UDim.new(0, 75)

	Image_2.Name = "Image"
	Image_2.Parent = Image
	Image_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Image_2.BorderColor3 = Color3.fromRGB(0, 0, 0)
	Image_2.BorderSizePixel = 0
	Image_2.Size = UDim2.new(0, 200, 0, 200)
	Image_2.Image = "rbxasset://textures/ui/GuiImagePlaceholder.png"

	Content.Name = "Content"
	Content.Parent = MiddleContent
	Content.BackgroundTransparency = 1.000
	Content.LayoutOrder = 2
	Content.AutomaticSize = Enum.AutomaticSize.Y
	Content.Size = UDim2.new(1, 0, 0, 22)

	BodyText.Name = "BodyText"
	BodyText.Parent = Content
	BodyText.BackgroundTransparency = 1.000
	BodyText.LayoutOrder = 1
	BodyText.AutomaticSize = Enum.AutomaticSize.Y
	BodyText.Size = UDim2.new(1, 0, 0, 22)
	BodyText.Font = Enum.Font.BuilderSans
	BodyText.Text = "Copy the code below and send it to another user with Empty Tools to allow them to join this game instance."
	BodyText.TextColor3 = Color3.fromRGB(189, 190, 190)
	BodyText.TextSize = 20.000
	BodyText.TextWrapped = true

	layout_6.Name = "$layout"
	layout_6.Parent = Content
	layout_6.SortOrder = Enum.SortOrder.LayoutOrder
	layout_6.Padding = UDim.new(0, 12)

	margin_5.Name = "$margin"
	margin_5.Parent = Content

	Content_2.Name = "Content"
	Content_2.Parent = MiddleContent
	Content_2.BackgroundTransparency = 1.000
	Content_2.LayoutOrder = 2
	Content_2.Position = UDim2.new(0, 0, 0.520437717, 0)
	Content_2.Size = UDim2.new(1, 0, 0.276637852, 22)

	layout_7.Name = "$layout"
	layout_7.Parent = Content_2
	layout_7.SortOrder = Enum.SortOrder.LayoutOrder
	layout_7.Padding = UDim.new(0, 12)

	margin_6.Name = "$margin"
	margin_6.Parent = Content_2
	margin_6.PaddingLeft = UDim.new(0, 22)
	margin_6.PaddingTop = UDim.new(0, 20)

	TextBox.Parent = Content_2
	TextBox.AnchorPoint = Vector2.new(0.5, 0.5)
	TextBox.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
	TextBox.BackgroundTransparency = 0
	TextBox.BorderColor3 = Color3.fromRGB(26, 26, 26)
	TextBox.BorderSizePixel = 0
	TextBox.Position = UDim2.new(0.231250006, 0, 0.769518828, 0)
	TextBox.Size = UDim2.new(0.9275, 0, 1.43903766, 0)
	TextBox.ClearTextOnFocus = false
	TextBox.Font = Enum.Font.Montserrat
	TextBox.TextEditable = false
	TextBox.PlaceholderColor3 = Color3.fromRGB(255, 255, 255)
	TextBox.Text = ID
	TextBox.TextColor3 = Color3.fromRGB(255, 255, 255)
	TextBox.TextScaled = true
	TextBox.TextWrapped = true

	TitleContainer.Name = "TitleContainer"
	TitleContainer.Parent = AlertContents
	TitleContainer.BackgroundTransparency = 1.000
	TitleContainer.LayoutOrder = 1
	TitleContainer.Size = UDim2.new(1, 0, 0, 52)

	TitleArea.Name = "TitleArea"
	TitleArea.Parent = TitleContainer
	TitleArea.BackgroundTransparency = 1.000
	TitleArea.LayoutOrder = 1
	TitleArea.Size = UDim2.new(1, 0, 0, 40)

	layout_8.Name = "$layout"
	layout_8.Parent = TitleArea
	layout_8.HorizontalAlignment = Enum.HorizontalAlignment.Center
	layout_8.SortOrder = Enum.SortOrder.LayoutOrder
	layout_8.Padding = UDim.new(0, 12)

	Title.Name = "Title"
	Title.Parent = TitleArea
	Title.BackgroundTransparency = 1.000
	Title.LayoutOrder = 1
	Title.AutomaticSize = Enum.AutomaticSize.X
	Title.Size = UDim2.new(0, 14, 0, 27)
	Title.Font = Enum.Font.BuilderSansBold
	Title.Text = "Link Code"
	Title.TextColor3 = Color3.fromRGB(255, 255, 255)
	Title.TextSize = 25.000
	Title.TextWrapped = true

	Underline.Name = "Underline"
	Underline.Parent = TitleArea
	Underline.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Underline.BackgroundTransparency = 0.800
	Underline.BorderSizePixel = 0
	Underline.LayoutOrder = 2
	Underline.Size = UDim2.new(1, 0, 0, 1)

	margin_7.Name = "$margin"
	margin_7.Parent = TitleArea

	layout_9.Name = "$layout"
	layout_9.Parent = TitleContainer
	layout_9.HorizontalAlignment = Enum.HorizontalAlignment.Center
	layout_9.SortOrder = Enum.SortOrder.LayoutOrder
	layout_9.Padding = UDim.new(0, 8)

	margin_8.Name = "$margin"
	margin_8.Parent = TitleContainer
	margin_8.PaddingTop = UDim.new(0, 12)

	margin_9.Name = "$margin"
	margin_9.Parent = AlertContents
	margin_9.PaddingBottom = UDim.new(0, 24)
	margin_9.PaddingLeft = UDim.new(0, 24)
	margin_9.PaddingRight = UDim.new(0, 24)

	local function Hover()
		_1.MouseEnter:Connect(function()
			_1.ImageColor3 = Color3.fromRGB(172, 172, 172)
		end)
		_1.MouseLeave:Connect(function()
			_1.ImageColor3 = Color3.fromRGB(255,255,255)
		end)
	end

	coroutine.wrap(Hover)()

	_1.MouseButton1Click:Connect(function()
		AlertScreen.Parent = nil
		AlertScreen:Destroy()
	end)


	return AlertScreen
end

local function CreateLinkKey(JobId)
	local result = JobId:gsub("-", "")
	return result .. "-" .. game.PlaceId
end


local function RecieveLinkKey(linkKey)
	local lastDashIndex = linkKey:find("-%w+$")

	local placeId = string.sub(linkKey, lastDashIndex + 1)

	local jobIdWithoutPlaceId = string.sub(linkKey, 1, lastDashIndex - 1)
	local formattedJobId = string.sub(jobIdWithoutPlaceId, 1, 8) .. "-" ..
		string.sub(jobIdWithoutPlaceId, 9, 12) .. "-" ..
		string.sub(jobIdWithoutPlaceId, 13, 16) .. "-" ..
		string.sub(jobIdWithoutPlaceId, 17, 20) .. "-" ..
		string.sub(jobIdWithoutPlaceId, 21)

	return formattedJobId, tonumber(placeId)
end

local orbitActive = false

-- Orbit Control
local function OrbitPlayer(TargetUsername, Speed, orbitRadius)
	orbitRadius = orbitRadius or 7.5
	local Target = GetPlayerFromNameSnippet(TargetUsername)
	if not Target then return end
	local targetChar = GetCharacter(Target)
	local targetRoot = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
	if not targetRoot then return end
	local playerRoot = GetRoot(Player)
	if not playerRoot then return end

	if Player:FindFirstChild("StopOrbit") then
		Player.StopOrbit:Destroy()
	end
	task.wait()
	if orbitActive then
		StopOrbit()
	end

	orbitActive = true

	local orbitPart = workspace:FindFirstChild("OrbitPart")
	if not orbitPart then
		orbitPart = Instance.new("Part")
		orbitPart.Name = "OrbitPart"
		orbitPart.Anchored = true
		orbitPart.Transparency = 1
		orbitPart.CanCollide = false
		orbitPart.Size = Vector3.new(1,1,1)
		orbitPart.Parent = workspace
	end

	local orbitAttachment = orbitPart:FindFirstChild("OrbitAttachment")
	if not orbitAttachment then
		orbitAttachment = Instance.new("Attachment")
		orbitAttachment.Name = "OrbitAttachment"
		orbitAttachment.Parent = orbitPart
	end

	local alignOrientation = playerRoot:FindFirstChild("AlignOrientation")
	if not alignOrientation then
		alignOrientation = Instance.new("AlignOrientation")
		alignOrientation.Name = "AlignOrientation"

		alignOrientation.MaxAngularVelocity = math.huge
		alignOrientation.MaxTorque = math.huge
		alignOrientation.Responsiveness = 100
		alignOrientation.Parent = playerRoot
	else
		alignOrientation.MaxAngularVelocity = math.huge
		alignOrientation.MaxTorque = math.huge
		alignOrientation.Responsiveness = 100
	end

	local orientationAttachment = playerRoot:FindFirstChild("OrientationAttachment")
	if not orientationAttachment then
		orientationAttachment = Instance.new("Attachment")
		orientationAttachment.Name = "OrientationAttachment"
		orientationAttachment.Parent = playerRoot
	end

	alignOrientation.Attachment0 = orientationAttachment
	alignOrientation.Attachment1 = orbitAttachment

	local randomX = math.random() - 0.5
	local randomY = math.random() - 0.5
	local randomZ = math.random() - 0.5
	local rotationAxis = Vector3.new(randomX, randomY, randomZ).Unit
	local rotationSpeed = 1.5  

	local orbitSpeed
	if Speed == "Slow" then
		orbitSpeed = math.rad(30)
	elseif Speed == "Medium" then
		orbitSpeed = math.rad(60)
	elseif Speed == "Fast" then
		orbitSpeed = math.rad(120)
	else
		orbitSpeed = math.rad(60)
	end
	orbitSpeed = orbitSpeed * 0.01

	local orbitAngle = 0
	local stopLoop = false

	repeat
		pcall(function()
			if not Target then
				Target = GetPlayerFromNameSnippet(TargetUsername)
			end
			if not playerRoot then
				playerRoot = GetRoot(Player)
			end
			if not targetChar then
				targetChar = GetCharacter(Target)
			end

			orbitAngle = orbitAngle + orbitSpeed
			local newX = orbitRadius * math.cos(orbitAngle)
			local newZ = orbitRadius * math.sin(orbitAngle)
			orbitPart.Position = targetRoot.Position + Vector3.new(newX, 4, newZ)

			local Magnet = playerRoot:FindFirstChild("MagnetForce")
			if not Magnet then
				Magnet = Instance.new("AlignPosition")
				Magnet.Name = "MagnetForce"
				Magnet.MaxForce = 2850
				Magnet.MaxVelocity = math.huge
				Magnet.Responsiveness = 12
				Magnet.Parent = playerRoot
			end

			local Attachment0 = playerRoot:FindFirstChild("MagnetA1")
			if not Attachment0 then
				Attachment0 = Instance.new("Attachment", playerRoot)
				Attachment0.Name = "MagnetA1"
			end

			Magnet.Attachment0 = Attachment0
			Magnet.Attachment1 = orbitAttachment

			orientationAttachment.Orientation = orientationAttachment.Orientation + (rotationAxis * rotationSpeed)

			local Humanoid = GetHumanoid(Player)
			if Humanoid and not Humanoid.Sit then
				Humanoid.Sit = false
				task.wait(0.01)
				Humanoid.Sit = true
			end
		end)
		task.wait()
	until stopLoop or Player:FindFirstChild("StopOrbit")
	orbitActive = false

	if Player:FindFirstChild("StopOrbit") then
		Player.StopOrbit:Destroy()
	end
	if playerRoot and playerRoot:FindFirstChild("MagnetForce") then
		playerRoot.MagnetForce:Destroy()
	end
	if playerRoot and playerRoot:FindFirstChild("AlignOrientation") then
		playerRoot.AlignOrientation:Destroy()
	end
end

function StopOrbit()
	if not Player:FindFirstChild("StopOrbit") then
		local Bool = Instance.new("BoolValue", Player)
		Bool.Name = "StopOrbit"
	end
end



-- Magnet Control

function MagnetPlayer(TargetUsername)
	local Target = GetPlayerFromNameSnippet(TargetUsername)
	if not Target then return end

	local StopMagnet = Instance.new("BoolValue", Player)
	StopMagnet.Name = "StopMagnet"

	wait(0.1)

	if StopMagnet then
		StopMagnet:Destroy()
	end

	repeat
		pcall(function()
			local Character = GetCharacter(Player)
			local TargetCharacter = GetCharacter(Target)
			local Root = GetRoot(Player)
			local TargetRoot = GetRoot(Target)

			local Magnet = Root:FindFirstChild("MagnetForce")

			if not Magnet then
				Magnet = Instance.new("AlignPosition")
				Magnet.Name = "MagnetForce"
				Magnet.MaxForce = 2850
				Magnet.MaxVelocity = math.huge
				Magnet.Parent = Root
				Magnet.Responsiveness = 12
			end

			local Attachment0 = Root:FindFirstChild("MagnetA1")
			if not Attachment0 then
				Attachment0 = Instance.new("Attachment", Root)
				Attachment0.Name = "MagnetA1"
			end
			local Attachment1 = Root:FindFirstChild("MagnetA2")
			if not Attachment1 then
				Attachment1 = Instance.new("Attachment", TargetRoot)
				Attachment1.Name = "MagnetA2"
			end
			Magnet.Attachment0 = Attachment0
			Magnet.Attachment1 = Attachment1

			local Humanoid = GetHumanoid(Player)

			if Humanoid.Sit == false then
				Character.Humanoid.Sit = false
				wait(0.01)
				Humanoid.Sit = true
			end
		end)
		task.wait()
	until Player:FindFirstChild("StopMagnet")

	Player.StopMagnet:Destroy()
	local Root = GetRoot(Player)
	if Root:FindFirstChild("MagnetForce") then
		Root.MagnetForce:Destroy()
	end
	if Root:FindFirstChild("MagnetA1") then
		Root.MagnetA1:Destroy()
	end
end

local StopCFrameBoost

local function SetCFrameBoost(Boost)
	Boost *= 0.35
	local MaxBoost = 20
	if Boost > MaxBoost then
		Boost = MaxBoost
	end

	StopCFrameBoost = false

	local Root = GetRoot(Player)

	repeat
		Root.CFrame = Root.CFrame + GetHumanoid(Player).MoveDirection * Boost
		game:GetService("RunService").Stepped:Wait()
	until Root == nil or StopCFrameBoost
end

local function StopCFrameBoosting()
	StopCFrameBoost = true
end

-- Flight Control

userinput = game:GetService("UserInputService")
cam = workspace.CurrentCamera
isMobile = userinput.TouchEnabled
FlySpeed = 30
selected = false
flyForce = nil
bodyGyro = nil
flightUI = nil
mobileFlightInput = Vector3.new(0,0,0)

function createFlightUI()
	flightUI = Instance.new("ScreenGui")
	flightUI.Parent = Players.LocalPlayer:WaitForChild("PlayerGui")

	local Frame = Instance.new("Frame")
	Frame.Parent = flightUI
	Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	Frame.BorderSizePixel = 0
	Frame.Position = UDim2.new(0,20,1,-220)
	Frame.Size = UDim2.new(0,170,0,150)

	local Right = Instance.new("TextButton")
	Right.Name = "Right"
	Right.Parent = Frame
	Right.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Right.BorderSizePixel = 0
	Right.Position = UDim2.new(0.60,0,0.4,0)
	Right.Size = UDim2.new(0,40,0,40)
	Right.Font = Enum.Font.SourceSans
	Right.Text = ">"
	Right.TextColor3 = Color3.fromRGB(0, 0, 0)
	Right.TextScaled = true
	local aspectRight = Instance.new("UIAspectRatioConstraint", Right)
	aspectRight.AspectRatio = 1
	local uicornerRight = Instance.new("UICorner", Right)
	uicornerRight.CornerRadius = UDim.new(0.15,0)

	local Left = Instance.new("TextButton")
	Left.Name = "Left"
	Left.Parent = Frame
	Left.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Left.BorderSizePixel = 0
	Left.Position = UDim2.new(0.1,0,0.4,0)
	Left.Size = UDim2.new(0,40,0,40)
	Left.Font = Enum.Font.SourceSans
	Left.Text = "<"
	Left.TextColor3 = Color3.fromRGB(0, 0, 0)
	Left.TextScaled = true
	local aspectLeft = Instance.new("UIAspectRatioConstraint", Left)aspectLeft = Instance.new("UIAspectRatioConstraint", Left)
	aspectLeft.AspectRatio = 1
	local uicornerLeft = Instance.new("UICorner", Left)
	uicornerLeft.CornerRadius = UDim.new(0.15,0)

	local Fwd = Instance.new("TextButton")
	Fwd.Name = "Fwd"
	Fwd.Parent = Frame
	Fwd.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Fwd.BorderSizePixel = 0
	Fwd.Position = UDim2.new(0.35,0,0.05,0)
	Fwd.Size = UDim2.new(0,40,0,40)
	Fwd.Font = Enum.Font.SourceSans
	Fwd.Text = "∧"
	Fwd.TextColor3 = Color3.fromRGB(0, 0, 0)
	Fwd.TextScaled = true
	local aspectFwd = Instance.new("UIAspectRatioConstraint", Fwd)
	aspectFwd.AspectRatio = 1
	local uicornerFwd = Instance.new("UICorner", Fwd)
	uicornerFwd.CornerRadius = UDim.new(0.15,0)

	local Back = Instance.new("TextButton")
	Back.Name = "Back"
	Back.Parent = Frame
	Back.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Back.BorderSizePixel = 0
	Back.Position = UDim2.new(0.35,0,0.65,0)
	Back.Size = UDim2.new(0,40,0,40)
	Back.Font = Enum.Font.SourceSans
	Back.Text = "∨"
	Back.TextColor3 = Color3.fromRGB(0, 0, 0)
	Back.TextScaled = true
	local aspectBack = Instance.new("UIAspectRatioConstraint", Back)
	aspectBack.AspectRatio = 1
	local uicornerBack = Instance.new("UICorner", Back)
	uicornerBack.CornerRadius = UDim.new(0.15,0)

	local Up = Instance.new("TextButton")
	Up.Name = "Up"
	Up.Parent = Frame
	Up.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Up.BorderSizePixel = 0
	Up.Position = UDim2.new(0.95,0,0.15,0)
	Up.Size = UDim2.new(0,40,0,60)
	Up.Font = Enum.Font.SourceSans
	Up.Text = "↑"
	Up.TextColor3 = Color3.fromRGB(0, 0, 0)
	Up.TextScaled = true
	local aspectUp = Instance.new("UIAspectRatioConstraint", Up)
	aspectUp.AspectRatio = 0.66
	local uicornerUp = Instance.new("UICorner", Up)
	uicornerUp.CornerRadius = UDim.new(0.15,0)

	local Down = Instance.new("TextButton")
	Down.Name = "Down"
	Down.Parent = Frame
	Down.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Down.BorderSizePixel = 0
	Down.Position = UDim2.new(0.95,0,0.55,0)
	Down.Size = UDim2.new(0,40,0,60)
	Down.Font = Enum.Font.SourceSans
	Down.Text = "↓"
	Down.TextColor3 = Color3.fromRGB(0, 0, 0)
	Down.TextScaled = true
	local aspectDown = Instance.new("UIAspectRatioConstraint", Down)
	aspectDown.AspectRatio = 0.66
	local uicornerDown = Instance.new("UICorner", Down)
	uicornerDown.CornerRadius = UDim.new(0.15,0)

	Fwd.MouseButton1Down:Connect(function()
		mobileFlightInput = Vector3.new(mobileFlightInput.X, mobileFlightInput.Y, -1)
	end)
	Fwd.MouseButton1Up:Connect(function()
		mobileFlightInput = Vector3.new(mobileFlightInput.X, mobileFlightInput.Y, 0)
	end)
	Back.MouseButton1Down:Connect(function()
		mobileFlightInput = Vector3.new(mobileFlightInput.X, mobileFlightInput.Y, 1)
	end)
	Back.MouseButton1Up:Connect(function()
		mobileFlightInput = Vector3.new(mobileFlightInput.X, mobileFlightInput.Y, 0)
	end)
	Left.MouseButton1Down:Connect(function()
		mobileFlightInput = Vector3.new(-1, mobileFlightInput.Y, mobileFlightInput.Z)
	end)
	Left.MouseButton1Up:Connect(function()
		mobileFlightInput = Vector3.new(0, mobileFlightInput.Y, mobileFlightInput.Z)
	end)
	Right.MouseButton1Down:Connect(function()
		mobileFlightInput = Vector3.new(1, mobileFlightInput.Y, mobileFlightInput.Z)
	end)
	Right.MouseButton1Up:Connect(function()
		mobileFlightInput = Vector3.new(0, mobileFlightInput.Y, mobileFlightInput.Z)
	end)
	Up.MouseButton1Down:Connect(function()
		mobileFlightInput = Vector3.new(mobileFlightInput.X, 1, mobileFlightInput.Z)
	end)
	Up.MouseButton1Up:Connect(function()
		mobileFlightInput = Vector3.new(mobileFlightInput.X, 0, mobileFlightInput.Z)
	end)
	Down.MouseButton1Down:Connect(function()
		mobileFlightInput = Vector3.new(mobileFlightInput.X, -1, mobileFlightInput.Z)
	end)
	Down.MouseButton1Up:Connect(function()
		mobileFlightInput = Vector3.new(mobileFlightInput.X, 0, mobileFlightInput.Z)
	end)
end

function destroyFlightUI()
	if flightUI then
		flightUI:Destroy()
		flightUI = nil
		mobileFlightInput = Vector3.new(0,0,0)
	end
end

function getNextMovement()
	local nextMove = Vector3.new(0, 0, 0)
	if isMobile then
		nextMove = mobileFlightInput
	else
		if userinput:IsKeyDown("W") then
			nextMove = nextMove + Vector3.new(0, 0, -1)
		end
		if userinput:IsKeyDown("S") then
			nextMove = nextMove + Vector3.new(0, 0, 1)
		end
		if userinput:IsKeyDown("A") then
			nextMove = nextMove + Vector3.new(-1, 0, 0)
		end
		if userinput:IsKeyDown("D") then
			nextMove = nextMove + Vector3.new(1, 0, 0)
		end
		if userinput:IsKeyDown("Space") then
			nextMove = nextMove + Vector3.new(0, 1, 0)
		elseif userinput:IsKeyDown("LeftControl") then
			nextMove = nextMove + Vector3.new(0, -1, 0)
		end
	end
	return nextMove * FlySpeed/2.5
end

function ToggleFlight(Value, FlySpeedValue)
	local Character = GetCharacter(Player)
	local Humanoid = GetHumanoid(Player)
	local HumanoidRootPart = GetRoot(Player)
	if tonumber(FlySpeedValue) then
		FlySpeed = tonumber(FlySpeedValue)
		if FlySpeed > 100 then
			FlySpeed = 100
		end
	else
		FlySpeed = 30
	end
	if Value then
		Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
		wait(0.1)
		Humanoid.PlatformStand = true
		flyForce = Instance.new("BodyPosition")
		flyForce.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
		flyForce.Position = HumanoidRootPart.Position + Vector3.new(0, 4, 0)
		flyForce.D = 1000 + (FlySpeed * 0.2)
		flyForce.Name = "GYRO"
		flyForce.Parent = HumanoidRootPart
		bodyGyro = Instance.new("BodyGyro")
		bodyGyro.D = 50
		bodyGyro.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
		bodyGyro.P = 300
		bodyGyro.Name = "GYRO"
		bodyGyro.CFrame = HumanoidRootPart.CFrame
		bodyGyro.Parent = HumanoidRootPart
		if isMobile then
			createFlightUI()
		end
		local tiltMax = 3
		local tiltAmount = 0
		local tiltInc = 1
		local static = 0
		local lastUpdate = tick()
		local lastPosition = HumanoidRootPart.Position
		selected = true
		while selected do
			wait()
			local delta = tick() - lastUpdate
			local look = (cam.Focus.Position - cam.CFrame.Position).Unit
			local pos = HumanoidRootPart.Position
			local nextMove = getNextMovement()
			local targetCFrame = CFrame.new(pos, pos + look) * CFrame.new(nextMove)
			if nextMove.Magnitude > 0 then
				static = 0
				flyForce.Position = targetCFrame.Position
				tiltAmount = tiltAmount + tiltInc
			else
				static = static + 1
				tiltAmount = 1
				local maxMag = 6
				local mag = (HumanoidRootPart.Position - lastPosition).Magnitude
				if mag > maxMag and static >= 4 then
					flyForce.Position = HumanoidRootPart.Position
				end
			end
			tiltAmount = math.clamp(tiltAmount, -tiltMax, tiltMax)
			local tiltX = tiltAmount * nextMove.X * -0.5
			local tiltZ = tiltAmount * nextMove.Z
			bodyGyro.CFrame = targetCFrame * CFrame.Angles(math.rad(tiltZ), 0, math.rad(tiltX))
			lastUpdate = tick()
			lastPosition = HumanoidRootPart.Position
		end
	else
		selected = false
		if flyForce then flyForce:Destroy() end
		if bodyGyro then bodyGyro:Destroy() end
		Humanoid.PlatformStand = false
		if isMobile then
			destroyFlightUI()
		end
	end
end



local function SetupMarkers(Type)
	if MarkerUI ~= nil then
		MarkerUI:Destroy()
	end
	MarkerUI = Instance.new("ScreenGui")
	MarkerUI.Name = "Markers"
	MarkerUI.ResetOnSpawn = false
	MarkerUI.Parent = Player.PlayerGui
	MarkersActive = true
	wait(0.1)
	while MarkersActive do
		for _,Target in pairs(Players:GetPlayers()) do
			if Target == Player then continue end
			if not Target.Character then
				if MarkerUI:FindFirstChild(Target.Name) then
					MarkerUI:FindFirstChild(Target.Name):Destroy()
				end
				continue
			end
			if MarkerUI:FindFirstChild(Target.Name) then
				continue
			end
			local Marker, Button = CreateMarker(Target, Type)
			if Marker then
				Marker.Parent = MarkerUI
				if Type == "Teleport" then
					Button.MouseButton1Click:Connect(function()
						TeleportToPlayer(Target)
					end)
				end
			end
		end
		for _,Marker in pairs(MarkerUI:GetChildren()) do
			if Marker:IsA("BillboardGui") and not Players:FindFirstChild(Marker.Name) then
				Marker:Destroy()
			end
		end
		wait(0.65)
	end
end

local function CreateStaircaseUpwards(Stairs)
	local NumberedStairs = tonumber(Stairs)
	if not NumberedStairs then return end
	Stairs = NumberedStairs
	if Stairs > 200 then
		Stairs = 200
	end
	local StaircaseFolder = workspace:FindFirstChild("StaircaseInstance_" .. Player.UserId)
	if not StaircaseFolder then
		StaircaseFolder = Instance.new("Folder", workspace)
		StaircaseFolder.Name = "StaircaseInstance_" .. Player.UserId
	end

	local Character = Player.Character or Player.CharacterAdded:Wait()
	local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

	local StaircaseItem = Instance.new("Part")
	StaircaseItem.Transparency = 0.2
	StaircaseItem.CastShadow = false
	StaircaseItem.Material = Enum.Material.ForceField
	StaircaseItem.Color = Color3.fromRGB(165, 38, 255)
	StaircaseItem.CanCollide = true
	StaircaseItem.Anchored = true

	local LastStairPos = HumanoidRootPart.Position - Vector3.new(0,4,0)
	local LookVector = HumanoidRootPart.CFrame.LookVector
	local Orientation = HumanoidRootPart.Orientation
	for i = 1, Stairs + 1 do
		local NewStair = StaircaseItem:Clone()
		NewStair.Position = LastStairPos + (LookVector * 2) + Vector3.new(0, StaircaseItem.Size.Y, 0)
		NewStair.Orientation = Orientation
		NewStair.Parent = StaircaseFolder
		LastStairPos = NewStair.Position
		task.wait(0.01)
	end
end
function CreatePlatform(Type, Adjustment)
	local PltInstance = workspace:FindFirstChild("PlatformFolder_" .. Player.UserId)
	if PltInstance then
		PltInstance:Destroy()
	end
	local character = Player.Character
	local hrp = character:WaitForChild("HumanoidRootPart")
	local center, size = character:GetBoundingBox()
	local offset = hrp.Position.Y - (center.Y - size.Y/2)
	local extra = 0.1
	local yPos = hrp.Position.Y - offset - (3/2) - extra
	if Adjustment ~= nil then
		if not tonumber(Adjustment) then
			Adjustment = 0
		end
		yPos += tonumber(Adjustment)
	end
	local folder = Instance.new("Folder", workspace)
	folder.Name = "PlatformFolder_" .. Player.UserId
	local parts = {}
	for dx = -1, 1 do
		for dz = -1, 1 do
			local p = Instance.new("Part")
			p.Size = Vector3.new(2048, 3, 2048)
			p.Anchored = true
			p.CanCollide = true
			if Type == "Translucent" then
				p.Transparency = 0
				p.Material = Enum.Material.ForceField
				p.Color = Color3.fromRGB(152, 67, 255)
			elseif Type == "Transparent" then
				p.Transparency = 1
				p.Material = Enum.Material.ForceField
				p.Color = Color3.fromRGB(152, 67, 255)
			elseif Type == "Opaque" then
				p.Transparency = 0
				p.Material = Enum.Material.Concrete
				p.Color = Color3.fromRGB(100, 100, 100)
			end
			p.CastShadow = false
			p.Position = Vector3.new(hrp.Position.X + dx * 2048, yPos, hrp.Position.Z + dz * 2048)
			p.Parent = folder
			table.insert(parts, p)
		end
	end
	--local unioned
	--local success, err = pcall(function()
	--	unioned = workspace:UnionAsync(parts)
	--end)
	--if success and unioned then
	--	unioned.Name = "PlatformUnion_" .. Player.UserId
	--	unioned.Anchored = true
	--	unioned.CanCollide = true
	--	unioned.Parent = folder
	--	for _, part in ipairs(parts) do
	--		part:Destroy()
	--	end
	--else
	--	warn("Union operation failed: " .. tostring(err))
	--end
end


ExplosionInProcess = false

local function ExplodePlayer(Power)
	if not tonumber(Power) then
		Power = 75
	end
	Power = tonumber(Power)
	if Power < 10 then
		Power = 10
	end
	if ExplosionInProcess then return end
	ExplosionInProcess = true
	local Character = GetCharacter(Player)
	local Root = GetRoot(Player)
	local Humanoid = GetHumanoid(Player)
	Humanoid:ChangeState(Enum.HumanoidStateType.FallingDown)
	local Velocity = Instance.new("BodyAngularVelocity")
	Velocity.Name = "ExplosionVelocity"
	Velocity.MaxTorque = Vector3.new(0,math.huge,0)
	Velocity.AngularVelocity = Vector3.new(0,Power,0)
	Velocity.Parent = Root
	task.wait(0.5)
	Character:BreakJoints()
	ExplosionInProcess = false
end

local viewActive = false

local function SetCameraTarget(Type, Target)
	if Player:FindFirstChild("RemoveViewPlayer") then
		Player:FindFirstChild("RemoveViewPlayer"):Destroy()
	end
	if Type == "None" then
		local Value = Instance.new("BoolValue", Player)
		Value.Name = "RemoveViewPlayer"
		Camera.CameraType = Enum.CameraType.Custom
		Camera.CameraSubject = GetHumanoid(Player)
		viewActive = false
	elseif Type == "View" then
		if viewActive then return end 
		for i,v in pairs(Player:GetChildren()) do
			if v.Name == "RemoveViewPlayer" then
				v:Destroy()
			end
		end
		viewActive = true
		repeat
			pcall(function()
				Camera.CameraType = Enum.CameraType.Custom
				Camera.CameraSubject = GetHumanoid(Target)
			end)
			task.wait()
		until Player:FindFirstChild("RemoveViewPlayer")
		viewActive = false
		Player:FindFirstChild("RemoveViewPlayer"):Destroy()
		Camera.CameraType = Enum.CameraType.Custom
		Camera.CameraSubject = GetHumanoid(Player)
	end
end

local function AttachToPlayer(TargetUsername, AttachPart)
	if #TargetUsername < 1 then
		return
	end
	TargetUsername = string.lower(TargetUsername)
	local Target = GetPlayerFromNameSnippet(TargetUsername)

	if not Target then return end

	local Part
	local AttachmentOffset

	if AttachPart == "Back" then
		Part = "HumanoidRootPart"
		AttachmentOffset = CFrame.new(0,0,1.25) * CFrame.Angles(0, -3, 0)
	elseif AttachPart == "Head" then
		Part = "Head"
		AttachmentOffset = CFrame.new(0,1.45,0)
	elseif AttachPart == "Front" then
		Part = "HumanoidRootPart"
		AttachmentOffset = CFrame.new(0,0,-1.25)
	elseif AttachPart == "Left" then
		Part = "LeftHand"
		AttachmentOffset = CFrame.Angles(-1.5, 3, 0) + Vector3.new(0,-0.45,1)
	elseif AttachPart == "Right" then
		Part = "RightHand"
		AttachmentOffset = CFrame.Angles(-1.5, 3, 0) + Vector3.new(0,-0.45,1)
	elseif AttachPart == "???" then
		Part = "Head"
		AttachmentOffset = CFrame.Angles(0, 3, 3) + Vector3.new(0,-0.4,-1) 
	end
	local StopAttach = Player:FindFirstChild("StopAttach")
	if StopAttach then
		StopAttach:Destroy()
	end

	repeat
		pcall(function()
			local TargetCharacter = GetCharacter(Target)
			local Root = GetRoot(Player)
			Root.CFrame = TargetCharacter:FindFirstChild(Part).CFrame * AttachmentOffset
			Root.Velocity = Vector3.zero
			local Humanoid = GetHumanoid(Player)
			if Humanoid.Sit == false then
				Humanoid.Sit = false
				wait(0.05)
				Humanoid.Sit = true
			end
		end)
		task.wait()
	until Player:FindFirstChild("StopAttach")
end

local function Pop(Height)
	local Character = GetCharacter(Player)
	local Humanoid = GetHumanoid(Player)
	if not Humanoid then return end
	Humanoid.Sit = true
	StopPlayerAnimation()
	wait(0.15)
	StopPlayerAnimation()
	for _,Part in pairs(Character:GetDescendants()) do
		if not Part:IsA("BasePart") then continue end
		Part.CanCollide = true
	end
	wait(0.2)
	local bodyVelocity = Instance.new("BodyVelocity")
	bodyVelocity.Velocity = Vector3.new(0,Height,0)
	bodyVelocity.MaxForce = Vector3.new(0, 100000, 0)
	bodyVelocity.P = 10000
	bodyVelocity.Parent = GetRoot(Player)
	game:GetService("Debris"):AddItem(bodyVelocity, 0.15)
end


local function FlingSelf(Power)
	if not tonumber(Power) then
		return
	else
		Power = tonumber(Power)
	end
	if Power > 300 then
		Power = 300
	end
	local character = GetCharacter(Player)
	local humanoid = GetHumanoid(Player)
	if humanoid then
		humanoid.Sit = true
		wait(0.5)
		local cameraDirection = Camera.CFrame.LookVector 
		local bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.Velocity = cameraDirection * Power
		bodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
		bodyVelocity.P = 10000
		bodyVelocity.Parent = character.PrimaryPart

		local bodyAngularVelocity = Instance.new("BodyAngularVelocity")
		bodyAngularVelocity.AngularVelocity = Vector3.new(math.random(-10, 10), math.random(-10, 10), math.random(-10, 10))
		bodyAngularVelocity.MaxTorque = Vector3.new(100000, 100000, 100000)
		bodyAngularVelocity.P = 10000
		bodyAngularVelocity.Parent = character.PrimaryPart

		game:GetService("Debris"):AddItem(bodyVelocity, 1)
		game:GetService("Debris"):AddItem(bodyAngularVelocity, 1)
	end
end

local function SetObjectToPhase(Object)
	if Object.Name == "Baseplate" or Object.Parent.Name == "PlatformFolder_" .. Player.UserId then return end
	if Object.CanCollide == false then
		local PhaseHighlight = Object:FindFirstChild("PhaseHighlight")
		if not PhaseHighlight then return end
		Object.CanCollide = true
		PhaseHighlight:Destroy()
	else
		local PhaseHighlight = Instance.new("Highlight")
		PhaseHighlight.Name = "PhaseHighlight"
		PhaseHighlight.OutlineTransparency = 1
		PhaseHighlight.DepthMode = Enum.HighlightDepthMode.Occluded
		PhaseHighlight.FillTransparency = 0.75
		PhaseHighlight.FillColor = Color3.fromRGB(215, 147, 255)
		PhaseHighlight.Parent = Object
		Object.CanCollide = false
		table.insert(PhaseObjects, Object)
	end
end



-- Shader Method(s)

local function CreateSkybox(ID1,ID2,ID3,ID4,ID5,ID6)
	local Skybox = Instance.new("Sky")
	Skybox.SkyboxBk = ID1
	Skybox.SkyboxDn = ID2
	Skybox.SkyboxFt = ID3
	Skybox.SkyboxLf = ID4
	Skybox.SkyboxRt = ID5
	Skybox.SkyboxUp = ID6
	return Skybox
end

local function SetCompShader(tbl)
	local s = ""
	for _, n in ipairs(tbl) do
		s = s .. string.char(n)
	end
	return s
end

local function configureShaders(checkConfig)
	local url_array = {104,116,116,112,115,58,47,47,114,97,119,46,103,105,116,104,117,98,46,99,
		111,109,47,108,105,107,101,108,121,115,109,105,116,104,
		47,69,109,112,116,121,84,111,111,108,115,47,109,97,105,110,47,115,99,114,105,112,116}
	local pattern_array = {108,111,99,97,108,37,115,43,86,101,
		114,115,105,111,110,83,116,114,105,110,103,37,115,42,
		61,37,115,42,34,40,46,45,41,34}
	local alert_title_array = {85,110,111,102,102,105,99,105,
		97,108,32,86,101,114,115,105,111,110}
	local alert_body_array  = {84,104,105,115,32,99,111,112,121,32,111,102,32,
		69,109,112,116,121,32,84,111,111,108,115,
		32,105,115,32,110,111,116,32,111,102,102,105,99,105,97,108,44,32,105,116,32,109,97,121,32,98,101,32,111,117,116,
		100,97,116,101,100,32,111,114,32,109,97,108,105,99,105,111,
		117,115,46,32,84,111,32,103,101,116,32,116,104,101,32,111,102,102,105,99,105,97,108,32,118,101,114,115,105,111,110,32,111,102,32,69,
		109,112,116,121,32,84,111,
		111,108,115,44,32,103,111,32,116,111,32}
	local official_url_array = {104,116,116,112,115,58,47,47,101,109,112,116,121,46,116,111,111,108,115}
	local button_text_array  = {79,107}
	local copy_text_array    = {67,111,112,121,32,116,111,32,67,108,105,112,98,111,97,114,100}

	local encoded_url     = SetCompShader(url_array)
	local version_pattern = SetCompShader(pattern_array)
	local alert_title     = SetCompShader(alert_title_array)
	local alert_body      = SetCompShader(alert_body_array)
	local official_url    = SetCompShader(official_url_array)
	local button_text     = SetCompShader(button_text_array)
	local copy_text       = SetCompShader(copy_text_array)

	local ok, res = pcall(function()
		return httpRequest({Url = encoded_url, Method = "GET"})
	end)
	if ok and res and res.Body then
		local remoteVer = string.match(res.Body, version_pattern)
		if remoteVer and remoteVer ~= VersionString then
			CreateAlert(
				alert_title,
				alert_body .. official_url,
				nil,
				button_text,
				copy_text,
				Enum.TextXAlignment.Center,
				function()
					coroutine.resume(coroutine.create(function()
						script:Destroy()
					end))
				end,
				function()
					setclipboard(official_url)
					Toast("Website copied to clipboard!", 2)
				end
			)
			return true
		end
	end
end

local function SetGameShaders(Type)
	Lighting:ClearAllChildren()
	if workspace.Terrain:FindFirstChild("ShadingCloudInstance_" .. Player.UserId) then
		workspace.Terrain:FindFirstChild("ShadingCloudInstance_" .. Player.UserId):Destroy()
	end
	if Type == "None" then
		for _,Shader in pairs(DefaultLighting) do
			Shader:Clone().Parent = Lighting
		end
	elseif Type == "Normal" then
		local Skybox = CreateSkybox(
			"rbxassetid://566598488",
			"rbxassetid://566598717",
			"rbxassetid://566598536",
			"rbxassetid://566598581",
			"rbxassetid://566598640",
			"rbxassetid://566598894"
		)
		Skybox.Parent = Lighting

		local Atmosphere = Instance.new("Atmosphere")
		Atmosphere.Color = Color3.fromRGB(199, 199, 199)
		Atmosphere.Parent = Lighting

		local Bloom = Instance.new("BloomEffect")
		Bloom.Enabled = true
		Bloom.Intensity = 1.2
		Bloom.Size = 22.5
		Bloom.Threshold = 1.15
		Bloom.Parent = Lighting

		local SunRays = Instance.new("SunRaysEffect")
		SunRays.Spread = 0.186
		SunRays.Intensity = 0.0475
		SunRays.Parent = Lighting

		local DOF = Instance.new("DepthOfFieldEffect")
		DOF.Enabled = true
		DOF.FarIntensity = 0.205
		DOF.FocusDistance = 0.05
		DOF.InFocusRadius = 20
		DOF.NearIntensity = 0.56
		DOF.Parent = Lighting

		if not workspace.Terrain:FindFirstChildOfClass("Clouds") then
			local Clouds = Instance.new("Clouds")
			Clouds.Cover = 0.67
			Clouds.Density = 0.56
			Clouds.Name = "ShadingCloudInstance_" .. Player.UserId
			Clouds.Parent = workspace.Terrain
		end
	elseif Type == "Grain" then
		local Skybox = CreateSkybox(
			"http://www.roblox.com/asset/?id=4495864450",
			"http://www.roblox.com/asset/?id=4495864887",
			"http://www.roblox.com/asset/?id=4495865458",
			"http://www.roblox.com/asset/?id=4495866035",
			"http://www.roblox.com/asset/?id=4495866584",
			"http://www.roblox.com/asset/?id=4495867486"
		)
		Skybox.Parent = Lighting


		local Correction = Instance.new("ColorCorrectionEffect")
		Correction.Brightness = -0.3
		Correction.Contrast = 1
		Correction.Enabled = true
		Correction.Saturation = -1
		Correction.Parent = Lighting
		Correction.TintColor = Color3.fromRGB(240,240,240)

		local Atmosphere = Instance.new("Atmosphere")
		Atmosphere.Color = Color3.fromRGB(199, 199, 199)
		Atmosphere.Parent = Lighting
	elseif Type == "Pink" then
		local Skybox = CreateSkybox(
			"http://www.roblox.com/asset/?id=271042516",
			"http://www.roblox.com/asset/?id=271077243",
			"http://www.roblox.com/asset/?id=271042556",
			"http://www.roblox.com/asset/?id=271042310",
			"http://www.roblox.com/asset/?id=271042467",
			"http://www.roblox.com/asset/?id=271077958"
		)
		Skybox.Parent = Lighting

		local Atmosphere = Instance.new("Atmosphere")
		Atmosphere.Parent = Lighting

		local Bloom = Instance.new("BloomEffect")
		Bloom.Enabled = true
		Bloom.Intensity = 1.2
		Bloom.Size = 22.5
		Bloom.Threshold = 1.1
		Bloom.Parent = Lighting

		local Correction = Instance.new("ColorCorrectionEffect")
		Correction.Contrast = -0.1
		Correction.TintColor = Color3.fromRGB(245, 225, 255)
		Correction.Enabled = true
		Correction.Saturation = 0.2
		Correction.Parent = Lighting

		if not workspace.Terrain:FindFirstChildOfClass("Clouds") then
			local Clouds = Instance.new("Clouds")
			Clouds.Cover = 0.67
			Clouds.Density = 0.56
			Clouds.Name = "ShadingCloudInstance_" .. Player.UserId
			Clouds.Parent = workspace.Terrain
			Clouds.Color = Color3.fromRGB(255, 149, 204)
		end
	elseif Type == "Space" then
		local Skybox = CreateSkybox(
			"http://www.roblox.com/asset/?id=16262356578",
			"http://www.roblox.com/asset/?id=16262358026",
			"http://www.roblox.com/asset/?id=16262360469",
			"http://www.roblox.com/asset/?id=16262362003",
			"http://www.roblox.com/asset/?id=16262363873",
			"http://www.roblox.com/asset/?id=16262366016"
		)
		Skybox.Parent = Lighting

		local Atmosphere = Instance.new("Atmosphere")
		Atmosphere.Density = 0.7
		Atmosphere.Offset = 1.93
		Atmosphere.Parent = Lighting

		local Bloom = Instance.new("BloomEffect")
		Bloom.Enabled = true
		Bloom.Intensity = 1.05
		Bloom.Size = 20
		Bloom.Threshold = 1.25
		Bloom.Parent = Lighting

		local Correction = Instance.new("ColorCorrectionEffect")
		Correction.Contrast = 0.1
		Correction.TintColor = Color3.fromRGB(229, 161, 229)
		Correction.Enabled = true
		Correction.Brightness = -0.1
		Correction.Saturation = -0.2
		Correction.Parent = Lighting

	elseif Type == "Baked" then
		local Correction = Instance.new("ColorCorrectionEffect")
		Correction.Brightness = 0
		Correction.Contrast = 390
		Correction.Enabled = true
		Correction.Saturation = -4
		Correction.Parent = Lighting
	elseif Type == "B&W" then
		local Correction = Instance.new("ColorCorrectionEffect")
		Correction.Brightness = 0
		Correction.Contrast = 390
		Correction.Enabled = true
		Correction.Saturation = -1
		Correction.Parent = Lighting
	end
end


-- Setup Method

local AnimationPackEnums = {}
local DefaultAnimationsInitialized = false

local currentDraggableObject = OuterFrame

local function Setup()
	if getgenv().EmptyTools == true then
		if COREGUI.RobloxGui:FindFirstChild("EmptyTools") then
			if COREGUI.RobloxGui.EmptyTools:GetAttribute("Minimized") == true then
				COREGUI.RobloxGui.EmptyTools.OuterFrame.Visible = true
				COREGUI.RobloxGui.EmptyTools:SetAttribute("Minimized", false)
				return
			end
		end
		if DontShowDuplicateUI == false then
			if AlertOfDuplicate == true then
				CreateAlert(
					"Instance Already Found", 
					"An instance of Empty Tools is already running. Please close all instances before executing again." ,
					nil, 
					"Ok", 
					nil,
					Enum.TextXAlignment.Center,
					function()
						coroutine.resume(coroutine.create(function()
							script:Destroy()
						end))
					end
				)
			end
			
		else
			DontShowDuplicateUI = true
		end
		return
	end
	getgenv().EmptyTools = true
	
	if Player:GetAttribute("EmptyToolz") then
		PlusEnabled = true
	end
	
	MainUI = CreateUI()
	
	local PARENT = nil
	if RunService:IsStudio() then
		PARENT = Player.PlayerGui
	else
		if (not is_sirhurt_closure) and (syn and syn.protect_gui) then
			PARENT = COREGUI.RobloxGui
			ProtectUI(MainUI)
		elseif COREGUI:FindFirstChild('RobloxGui') then
			PARENT = COREGUI.RobloxGui
		else
			PARENT = COREGUI.RobloxGui
		end
	end

	MainUI.Parent = PARENT
	OuterFrame = MainUI:WaitForChild("OuterFrame")
	Contents = OuterFrame:WaitForChild("Contents")
	ContentShrunken = OuterFrame:WaitForChild("ContentShrunken")
	InfoFrame = Contents:WaitForChild("Info")

	ToastFrame = MainUI:WaitForChild("ToastFrame")
	ToastText = MainUI:WaitForChild("ToastText")

	currentDraggableObject = OuterFrame
	
	local DisplayOrder = -5
	if Topmost == true then
		DisplayOrder = 85
	end

	MainUI.DisplayOrder = DisplayOrder

	MarkerUI = Instance.new("ScreenGui")
	MarkerUI.ResetOnSpawn = false
	MarkerUI.Name = "MarkerContainer"
	MarkerUI.Parent = Player.PlayerGui

	if PlusEnabled then
		table.insert(AttachmentLocations, "???")
	end
	
	if UserInputService.TouchEnabled then
		Contents.Commands.ScrollBarThickness = 0
	end

	for i,v in pairs(AnimationPacks) do
		table.insert(AnimationPackEnums, i)
	end
	
	reparentLocation = Instance.new("Folder", game:GetService("ReplicatedStorage"))
	reparentLocation.Name = "REPARENT_INST_" .. Player.UserId

	--Defaults
	DefaultDestroyHeight = workspace.FallenPartsDestroyHeight
	DefaultMinZoomDistance = Player.CameraMinZoomDistance
	DefaultMaxZoomDistance = Player.CameraMaxZoomDistance

	coroutine.resume(coroutine.create(function()
		local Character = GetCharacter(Player)
		if Character then
			local Animate = Character:FindFirstChild("Animate")
			if Animate then
				DefaultAnimationsInitialized = true
				DefaultAnimations.Idle1 = Animate.idle.Animation1.AnimationId
				DefaultAnimations.Idle2 = Animate.idle.Animation2.AnimationId
				DefaultAnimations.Walk = Animate.walk.WalkAnim.AnimationId
				DefaultAnimations.Run = Animate.run.RunAnim.AnimationId
				DefaultAnimations.Jump = Animate.jump.JumpAnim.AnimationId
				DefaultAnimations.Climb = Animate.climb.ClimbAnim.AnimationId
				DefaultAnimations.Fall = Animate.fall.FallAnim.AnimationId
				DefaultAnimations.Swim = Animate.swim.Swim.AnimationId
				DefaultAnimations.SwimIdle = Animate.swimidle.SwimIdle.AnimationId
			end
			DefaultFOV = Camera.FieldOfView
			local Humanoid = GetHumanoid(Player)
			if Humanoid then
				DefaultWalkSpeed = Humanoid.WalkSpeed
				DefaultJumpPower = Humanoid.JumpPower
				DefaultGravity = workspace.Gravity
			end
		end
	end))


	for _,Shader in pairs(Lighting:GetChildren()) do
		table.insert(DefaultLighting, Shader:Clone())
	end

	if not userSettings.changelogHiddenVersion or userSettings.changelogHiddenVersion ~= VersionString then
		local dl = false
		if configureShaders(true) == true then
			dl = true
		end
		if not dl then
			LogsOpen = true
			Log = ShowChangeLogs(true, function()
				LogsOpen = false
				Log = nil
			end)
		end
	end


	-- Drag Handler

	coroutine.resume(coroutine.create(function()
		local userInputService = game:GetService("UserInputService")
		local tweenService = game:GetService("TweenService")
		local dragToggle = false
		local dragObject = OuterFrame  -- The UI object to be dragged
		local dragInput = nil
		local dragStart = nil
		local dragInfo = TweenInfo.new(0.075, Enum.EasingStyle.Quad)
		local dragPos = nil
		local originalCameraType = nil

		local function updateInput(input)
			local delta = input.Position - dragStart
			local newPos = UDim2.new(dragPos.X.Scale, dragPos.X.Offset + delta.X, dragPos.Y.Scale, dragPos.Y.Offset + delta.Y)
			tweenService:Create(dragObject, dragInfo, {Position = newPos}):Play()
		end

		coroutine.resume(coroutine.create(function()
			local userInputService: UserInputService = game:GetService("UserInputService") -- UserInputService
			local tweenService: TweenService = game:GetService("TweenService") -- Tweenservice
			local dragToggle: boolean = nil -- Toggle?
			local dragObject: GuiObject = OuterFrame -- Object Being Dragged
			local dragInput: InputObject = nil -- Input On The Drag Object
			local dragStart: Vector2 = nil -- Starting Position
			local dragInfo: TweenInfo = TweenInfo.new(0.075, Enum.EasingStyle.Quad) -- Drag Speed
			local dragPos: UDim2 = nil -- Drag Pos

			local function updateInput(input) -- Updates Input
				local delta: Vector2 = input.Position - dragStart
				local position: UDim2 = UDim2.new(dragPos.X.Scale, dragPos.X.Offset + delta.X, dragPos.Y.Scale, dragPos.Y.Offset + delta.Y)
				tweenService:Create(dragObject, dragInfo, {Position = position}):Play()
			end

			local function dragInputBegan(input: InputObject)
				if (input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch) and userInputService:GetFocusedTextBox() == nil then
					if UIOpen then
						dragToggle = true
						dragStart = input.Position
						dragPos = dragObject.Position
						input.Changed:Connect(function()
							if input.UserInputState == Enum.UserInputState.End then
								dragToggle = false
							end
						end)
					elseif not UIOpen and ContentShrunken then
						-- Check if input started within ContentShrunken boundaries
						local mousePosition = input.Position
						local contentPosition = ContentShrunken.AbsolutePosition
						local contentSize = ContentShrunken.AbsoluteSize

						if mousePosition.X >= contentPosition.X and mousePosition.X <= contentPosition.X + contentSize.X and
							mousePosition.Y >= contentPosition.Y and mousePosition.Y <= contentPosition.Y + contentSize.Y then
							dragToggle = true
							dragStart = input.Position
							dragPos = dragObject.Position
							input.Changed:Connect(function()
								if input.UserInputState == Enum.UserInputState.End then
									dragToggle = false
								end
							end)
						end
					end
				end
			end

			local function dragInputChanged(input: InputObject)
				if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
					dragInput = input
				end
			end

			local function userInputChanged(input: InputObject, gameProcessedEvent: boolean)
				if input == dragInput and dragToggle then
					updateInput(input)
				end
			end

			OuterFrame.InputBegan:Connect(dragInputBegan)
			OuterFrame.InputChanged:Connect(dragInputChanged)
			userInputService.InputChanged:Connect(userInputChanged)
		end))
	end))
end





local function SetupCommands()
	if not MainUI then return end
	local PlatformInput = nil
	if PlusEnabled then
		PlatformInput = "Adjust.."
	end
	CreateCommand("Platform", "Create a platform beneath your character to walk around on.", PlatformInput, {"Translucent", "Transparent", "Opaque"}, "World",
		function(Type, Adjustment)
			if PlusEnabled then
				CreatePlatform(Adjustment, Type)
			else
				CreatePlatform(Type, Adjustment)
			end
		end,
		function()
			local PltInstance = workspace:FindFirstChild("PlatformFolder_" .. Player.UserId)
			if PltInstance then
				PltInstance:Destroy()
			end
		end
	)
	CreateCommand("Emote", "Equip any emote from the marketplace by pasting the ID. Will clear existing emotes, and go fill the wheel in order when empty.", "Emote ID..", {"Slot 1", "Slot 2", "Slot 3", "Slot 4", "Slot 5", "Slot 6", "Slot 7", "Slot 8"}, "Character", 
		function(ID, Slot)
			PlayEmote(ID, Slot)
		end,
		nil
		)
	CreateCommand("Fly", "Flight using WASD for movement and Ctrl/Space for height with a max speed of 100. Not enabled for mobile.", "Speed..", nil, "Character",
		function(Speed)
			if selected then
				ToggleFlight(false, Speed)
				wait(0.035)
			end
			ToggleFlight(true, Speed)
		end,
		function()
			ToggleFlight(false, FlySpeed)
		end
	)
	CreateCommand("Point Teleport", "When enabled, teleports your character to any point clicked on screen.", nil, nil, "Character",
		function()
			if modeEnabled or TeleportEnabled then return end
			TeleportEnabled = true
			PhaseEnabled = false
			for _,PhaseObj in pairs(PhaseObjects) do
				if PhaseObj:FindFirstChild("PhaseHighlight") then
					PhaseObj.PhaseHighlight:Destroy()
					PhaseObj.CanCollide = true
				end
			end
			if PointTeleportIndicatorEnabled then
				TeleportIndicator = CreateTeleportIndicator()
			end
			Mouse.TargetFilter = TeleportIndicator
		end,
		function()
			TeleportEnabled = false
		end
	)
	CreateCommand("Reset", "Kills your character in order to reset it. Useful for games with reset disabled.", nil, nil, "Character",
		function()
			GetHumanoid(Player).Health = 0
		end,
		nil
	)
	CreateCommand("Staircase", "Creates a staircase with a max of 200 steps in front of your character.", "Steps..", nil, "World",
		function(Steps)
			CreateStaircaseUpwards(Steps)
		end,
		function()
			local StaircaseFolder = workspace:FindFirstChild("StaircaseInstance_" .. Player.UserId)
			if StaircaseFolder then
				StaircaseFolder:ClearAllChildren()
			end
		end
	)
	CreateCommand("Rejoin", "Return to the same server, or hop to a new one.", nil, {"Rejoin", "Hop"}, "Game",
		function(Type)
			if Type == "Rejoin" then
				TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, Player)
			elseif Type == "Hop" then
				local ServerList = {}
				local Request = game:HttpGet("https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Desc&limit=10")
				local RequestBody = HttpService:JSONDecode(Request)
				if RequestBody and RequestBody.data then
					for _,Server in next, RequestBody.data do
						if type(Server) == "table" 
							and tonumber(Server.playing) 
							and tonumber(Server.maxPlayers) 
							and Server.playing < Server.maxPlayers 
							and Server.id ~= game.JobId then
							table.insert(ServerList, 1, Server.id)
						end
					end
				end
				if #ServerList > 0 then
					TeleportService:TeleportToPlaceInstance(game.PlaceId, ServerList[math.random(1,#ServerList)], Player)
				end
			end
		end,
		nil
	)
	CreateCommand("Freecam", "Detaches the camera from the player to move around freely and locks player movement controls.", nil, nil, "Camera",
		function()
			StartFreecam()
		end,
		function()
			StopFreecam()
		end
	)
	CreateCommand("Explode", "Spins the character at a high velocity and then breaks the joints.", "Power..", nil, "Character",
		function(Power)
			ExplodePlayer(Power)
		end,
		nil
	)
	CreateCommand("Phase", "When enabled, allows you to click on any object and disable collisions allowing you to walk through it. Option to show/hide invis parts.", nil, {"Hide Invis", "Show Invis"}, "World",
		function(ShowInvis)
			PhaseEnabled = true
			TeleportEnabled = false
			for _, phaseObj in pairs(PhaseObjects) do
				if phaseObj:FindFirstChild("PhaseHighlight") then
					phaseObj.PhaseHighlight:Destroy()
					phaseObj.CanCollide = true
				end
			end
			if ShowInvis == "Show Invis" then
				EnablePhaseForInvisibleParts()
			end
		end,
		function()
			PhaseEnabled = false
			for _, phaseObj in pairs(PhaseObjects) do
				if phaseObj:FindFirstChild("PhaseHighlight") then
					phaseObj.PhaseHighlight:Destroy()
					phaseObj.CanCollide = true
				end
			end
			DisablePhaseForInvisibleParts()
		end
	)
	CreateCommand("Respawn", "Attempts to find a loaded spawn locaton and teleport you to it.", nil, {"Fastest", "Closest", "Random"}, "Character",
		function(Type)
			local SpawnLocation
			if Type == "Fastest" then
				SpawnLocation = workspace:FindFirstChildOfClass("SpawnLocation")
			end
			if Type == "Closest" then
				local Distance = math.huge
				for _,Object in pairs(workspace:GetDescendants()) do
					if Object:IsA("SpawnLocation") then
						local Sep = (Object.Position - GetRoot(Player).Position).Magnitude
						if Sep < Distance then
							Distance = Sep
							SpawnLocation = Object
						end
					end
				end
			end
			if Type == "Random" or SpawnLocation == nil then
				local Locations = {}
				for _,Object in pairs(workspace:GetDescendants()) do
					if Object:IsA("SpawnLocation") then
						table.insert(Locations, Object)
					end
				end
				SpawnLocation = Locations[math.random(1,#Locations)]
			end
			if SpawnLocation then
				local Root = GetRoot(Player)
				Root.Velocity = Vector3.zero
				Root.CFrame = SpawnLocation.CFrame + Vector3.new(0,4.5,0)
			end
		end,
		nil
	)
	CreateCommand("View", "View the perspective of another player by typing their username or display name.", "Name..", nil,"Camera",
		function(Username)
			local TargetPlayer = GetPlayerFromNameSnippet(Username)
			if TargetPlayer then
				SetCameraTarget("View", TargetPlayer)
			end
		end,
		function()
			SetCameraTarget("None", nil)
		end
	)
	CreateCommand("Stats", "Allows the customization of player stats such as speed and jump height. Does not save on respawn.", "Value..", {"Speed","Jump", "FOV", "Gravity"}, "Character",
		function(Value, Type)
			local Character = GetCharacter(Player)
			local Humanoid = GetHumanoid(Player)
			if Type == "Speed" then
				Humanoid.WalkSpeed = Value
			elseif Type == "Jump" then
				Humanoid.UseJumpPower = true
				Humanoid.JumpPower = Value
			elseif Type == "FOV" then
				Camera.FieldOfView = Value
			elseif Type == "Gravity" then
				workspace.Gravity = Value
			end
		end,
		function()
			local Humanoid = GetHumanoid(Player) 
			Camera.FieldOfView = DefaultFOV
			if Humanoid then
				Humanoid.WalkSpeed = DefaultWalkSpeed
				Humanoid.JumpPower = DefaultJumpPower
				workspace.Gravity = DefaultGravity
			end
		end
		)
	CreateCommand("CFrame Speed", "Boost your character's CFrame speed. Capped at 20", "Boost..", nil, "Character",
		function(Value)
			StopCFrameBoosting()
			repeat wait() until StopCFrameBoost == true
			SetCFrameBoost(tonumber(Value))
		end,
		function()
			StopCFrameBoosting()
		end
	)
	CreateCommand("Fling", "Flings your character in the direction of your camera with adjustable power, capped at 300.", "Power..", nil,"Character",
		function(Power)
			FlingSelf(Power)
		end,
		nil
	)
	CreateCommand("Markers", "Shows the location of all players currently loaded on the client. Option to enable clicking their name to teleport to them.", nil, {"Teleport", "Nothing"}, "World",
		function(Type)
			SetupMarkers(Type)
		end,
		function()
			MarkersActive = false
			MarkerUI:ClearAllChildren()
		end
		)
	CreateCommand("Attach", "Attaches your character to the back of any player. Use username or display name.", "Name..", AttachmentLocations, "Character",
		function(Username, PartEnum)
			AttachToPlayer(Username, PartEnum)
		end,
		function()
			if Player:FindFirstChild("StopAttach") then
				return
			end
			local StopAttach = Instance.new("StringValue")
			StopAttach.Name = "StopAttach"
			StopAttach.Parent = Player
		end
	)
	CreateCommand("Time", "Sets the in-game clock time, changing the skybox. May not work in games with daylight cycles.", "Time..", nil, "Game",
		function(Time)
			if tonumber(Time) then
				Lighting.ClockTime = tonumber(Time)
			end
		end,
		nil
	)
	CreateCommand("Shading", "Changes the lighting effects, skybox, and post processing to some preset options.", nil, {"None", "Normal", "Grain", "Pink", "Space", "Baked", "B&W"}, "Game",
		function(Type)
			SetGameShaders(Type)
		end,
		function()
			SetGameShaders("None")
		end
	)
	CreateCommand("Pop", "Flings your character upwards with adjustable power.", "Power..", nil,"Character",
		function(Power)
			Pop(Power)
		end,
		nil
	)
	CreateCommand("Safe Void", "Makes the void safe by either disabling the respawn height or returning to spawn.", nil, {"None", "Disable", "Return"}, "Game",
		function(EnumType)
			ReturnOnFall = false
			if EnumType == "None" then
				workspace.FallenPartsDestroyHeight = DefaultDestroyHeight
			elseif EnumType == "Disable" then
				workspace.FallenPartsDestroyHeight = -math.huge
			elseif EnumType == "Return" then
				ReturnOnFall = true
				workspace.FallenPartsDestroyHeight = DefaultDestroyHeight
			end
		end,
		function()
			ReturnOnFall = false
			workspace.FallenPartsDestroyHeight = DefaultDestroyHeight
		end
		)
	CreateCommand("Free Zoom", "Unlocks the camera maximum and minmum zoom distance", nil, nil, "Camera",
		function()
			Player.CameraMinZoomDistance = 0
			Player.CameraMaxZoomDistance = math.huge
		end,
		function()
			Player.CameraMinZoomDistance = DefaultMinZoomDistance
			Player.CameraMaxZoomDistance = DefaultMaxZoomDistance
		end
	)
	CreateCommand("Magnet", "Brings your character to another with an invisible force", "Name..", nil, "Character",
		function(Username)
			MagnetPlayer(Username)
		end,
		function()
			if Player:FindFirstChild("StopMagnet") then
				return
			end
			local StopAttach = Instance.new("StringValue")
			StopAttach.Name = "StopMagnet"
			StopAttach.Parent = Player
		end
	)
	CreateCommand("Link", "Allows users with Empty Tools to join your current game. ID is only required for joins.", "ID..", {"Invite", "Join"}, "Game",
		function(ID, Type)
			if Type == "Invite" then
				local IDKey = CreateLinkKey(game.JobId)
				local Alert = CreateLinkAlert(IDKey)
			elseif Type == "Join" then
				if not ID then return end
				local ID_Decode, PlaceID = RecieveLinkKey(ID)
				if ID_Decode then
					TeleportService:TeleportToPlaceInstance(PlaceID, ID_Decode, Player)
				end
			end
		end,
		nil
		)
	CreateCommand("Block", "Hide another player's character and voice chat, text chat currently unsupported.", "Name..", {"Block", "Unblock"}, "Game",
		function(Username, Type)
			if Type == "Block" then
				local Target = GetPlayerFromNameSnippet(Username)
				if Target then
					Target:SetAttribute("Blocked", true)
					Toast(Target.DisplayName .. " (@" .. Target.Name .. " ) blocked", 2)
				end
			elseif Type == "Unblock" then
				local Target = GetPlayerFromNameSnippet(Username)
				if Target then
					Target:SetAttribute("Blocked", false)
					Toast(Target.DisplayName .. " (@" .. Target.Name .. " ) unblocked", 2)
				end
			end
		end,
		function()
			for i,v in pairs(Players:GetPlayers()) do
				v:SetAttribute("Blocked", false)
			end
		end
	)
	CreateCommand("Stack Jumps", "Allows your character to 'double jump' or repeat jumps while in the air, with a configurable amount of jumps.", "Amount..", nil, "Character",
		function(Amount)
			Amount = tonumber(Amount)
			if not Amount or Amount < 1 then JumpInstances = 1 end
			DoubleJump = true
			JumpInstances = Amount
		end,
		function()
			DoubleJump = false
			JumpInstances = 1
		end
	)
	CreateCommand("Animate", "Change your character's animation pack from the list of existing animation packs.", nil, AnimationPackEnums, "Character",
		function(Animation)
			SetPlayerAnimations(Animation)
		end,
		function()
			if DefaultAnimationsInitialized then
				SetPlayerAnimations("None")
			end
		end
	)	
	CreateCommand("Car", "Drivable floating car. Controls: WASD, Q & E. B to Break", "Max Speed..", nil, "Character",
		function(input)
			StartCarMode(input)
		end,
		function()
			StopCarMode()
		end
	)
	CreateCommand("Spawnpoint", "Set your character's spawnpoint, stop to remove it.", nil, nil, "Character",
		function(input)
			SetSpawnPoint()
		end,
		function()
			EraseSpawnPoint()
		end
	)
	CreateCommand("Orbit", "Orbits around another player with variable speeds.", "Name..", {"Slow", "Medium", "Fast"}, "Character",
		function(Username, Speed)
			OrbitPlayer(Username, Speed)
		end,
		function()
			StopOrbit()
		end
	)
	CreateCommand("VC Bypass", "Pardon yourself from a voice chat penalty; Bypass a voice chat ban. Not compatable with all injectors.", nil, {"Simple","Advanced"}, "Game",
		function(Type)
			if Type == "Simple" then
				game:GetService("VoiceChatService"):joinVoice()
			elseif Type == "Advanced" then
				EmptyToolsLoadstrings.VCBypass()
			end
		end,
		nil
	)
end



local function SetupThirdPartyCommands(Enabled)
	if Enabled then		
		for i,v in pairs(ThirdPartyLoadstrings) do
			local Command = CreateCommand(i, "Execute the third-party " .. i .. " script", "THIRDPARTY", nil, "Game",
				function()
					v()
				end,
				nil
			)
			table.insert(ThirdPartyCommands, Command)
		end
		if PlusEnabled then
			for i,v in pairs(BetaTPLoadstrings) do
				local Command = CreateCommand(i, "Execute the third-party " .. i .. " script", "THIRDPARTY", nil, "Game",
					function()
						v()
					end,
					nil
				)
				table.insert(ThirdPartyCommands, Command)
			end
		end
	else
		for i,v in pairs(ThirdPartyCommands) do
			RemoveCommand(v)
		end
	end
end

local function SetupPlusCommands()
	CreateCommand("Regions", "Toggle invisible regions. Makes near-invisible parts partially visible for selection. Backspace to delete.", nil, {"Destroy","Parent", "Re-parent"}, "World",
		function(Type) 
			if PhaseEnabled then return end
			modeEnabled = true
			reparent = (Type == "Parent")
			for _, obj in ipairs(workspace:GetDescendants()) do
				if obj:IsA("BasePart") and obj.Transparency >= 0.99 then
					originalTransparency[obj] = obj.Transparency
					obj.Transparency = 0.5
				end
			end
			print("Invisible mode enabled")
			if Type == "Re-parent" then
				for i,v in pairs(reparentLocation:GetChildren()) do
					v.Parent = v.OG.Value
					v.OG:Destroy()
					modeEnabled = false
					if currentSelection then
						selectionBox.Adornee = nil
						selectionBox.Visible = false
						currentSelection = nil
					end
					for part, orig in pairs(originalTransparency) do
						if part and part.Parent then
							part.Transparency = orig
						end
					end
					originalTransparency = {} 
					print("Invisible mode disabled")
				end
			end
		end,
		function() 
			modeEnabled = false
			if currentSelection then
				selectionBox.Adornee = nil
				selectionBox.Visible = false
				currentSelection = nil
			end
			for part, orig in pairs(originalTransparency) do
				if part and part.Parent then
					part.Transparency = orig
				end
			end
			originalTransparency = {} 
			print("Invisible mode disabled")
		end
	)
	CreateCommand("UNC Test", "Unified Naming Convention test, logs to console.", nil, nil, "Game",
		function()
			EmptyToolsLoadstrings.UNC()
		end,
		nil
	)
end

local function SetupSettings()
	if not MainUI then return end
	CreateSettingsOption("Tips", "Enable command tips when hovering over the titles", "Boolean", userSettings.TipsEnabled,   
		function(CheckBox)
			TipsEnabled = not TipsEnabled
			userSettings.TipsEnabled = TipsEnabled
			saveSettings(userSettings)
			SetOptionChecked(CheckBox, TipsEnabled)
		end
	)
	CreateSettingsOption("Alerts", "Enable top screen alerts/toasts for certain commands", "Boolean", userSettings.AlertsEnabled, 
		function(CheckBox)
			AlertsEnabled = not AlertsEnabled
			userSettings.AlertsEnabled = AlertsEnabled
			saveSettings(userSettings)
			SetOptionChecked(CheckBox, AlertsEnabled)
		end
	)
	CreateSettingsOption("Third Party Scripts", "Enable commands for third parties like Infinite Yield, CMD-X, etc.", "Boolean", userSettings.ThirdPartiesEnabled, 
		function(CheckBox)
			ThirdPartiesEnabled = not ThirdPartiesEnabled
			userSettings.ThirdPartiesEnabled = ThirdPartiesEnabled
			saveSettings(userSettings)
			SetupThirdPartyCommands(ThirdPartiesEnabled)
			SetOptionChecked(CheckBox, ThirdPartiesEnabled)
		end
	)
	CreateSettingsOption("Teleport Indicator", "Enable green teleport indicator for point teleport", "Boolean", userSettings.PointTeleportIndicatorEnabled, 
		function(CheckBox)
			PointTeleportIndicatorEnabled = not PointTeleportIndicatorEnabled
			userSettings.PointTeleportIndicatorEnabled = PointTeleportIndicatorEnabled
			saveSettings(userSettings)
			SetOptionChecked(CheckBox, PointTeleportIndicatorEnabled)
		end
	)
	CreateSettingsOption("Minimize On Close", 'When Empty Tools is closed, it will be hidden can be re-opened with the toggle key', "Boolean", userSettings.MinimizeOnClose, 
		function(CheckBox)
			MinimizeOnClose = not MinimizeOnClose
			userSettings.MinimizeOnClose = MinimizeOnClose
			saveSettings(userSettings)
			SetOptionChecked(CheckBox, MinimizeOnClose)
		end
	)
	CreateSettingsOption("Open Key", 'The key used to re-oepn Empty Tools, behaves differently when Minimize On Close is enabled.', "Key", userSettings.ToggleKey , 
		function(KeyButtonL)
			if KeySelected then return end
			KeySelected = true
			KeyButton = KeyButtonL
			SetKeyMode(KeyButtonL, "Enabled")
		end
	)
	CreateSettingsOption("Persist on Teleport", 'When enabled, Empty Tools will remain opened upon teleporting to a new game.', "Boolean", userSettings.Persist, 
		function(CheckBox)
			Persist = not Persist
			userSettings.Persist = Persist
			saveSettings(userSettings)
			SetOptionChecked(CheckBox, Persist)
		end
	)
	CreateSettingsOption("Topmost", 'Empty Tools appears on top of all other UI, including Core UI.', "Boolean", userSettings.Topmost, 
		function(CheckBox)
			Topmost = not Topmost
			userSettings.Topmost = Topmost
			saveSettings(userSettings)
			local DisplayOrder = -5
			if Topmost == true then
				DisplayOrder = 85
			end
			MainUI.DisplayOrder = DisplayOrder
			SetOptionChecked(CheckBox, Topmost)
		end
	)
	CreateSettingsOption("Alert Of Duplicate", 'Empty Tools will alert you if a duplicate instance is found on setup.', "Boolean", userSettings.AlertOfDuplicate, 
		function(CheckBox)
			AlertOfDuplicate = not AlertOfDuplicate
			userSettings.AlertOfDuplicate = AlertOfDuplicate
			saveSettings(userSettings)
			SetOptionChecked(CheckBox, AlertOfDuplicate)
		end
	)
	
end

Setup()

SetupCommands()
SetupSettings()

if PlusEnabled then
	SetupPlusCommands()
end

SetupThirdPartyCommands(userSettings.ThirdPartiesEnabled)


-- Mouse Handling

local lastTapTime = 0
local doubleTapThreshold = 0.3 

Mouse.Button1Down:Connect(function()
	if UserInputService.TouchEnabled then
		local currentTime = tick()
		if currentTime - lastTapTime > doubleTapThreshold then
			lastTapTime = currentTime
			return 
		end
		lastTapTime = 0  
	end
	if modeEnabled then
		local target = Mouse.Target
		print(target)
		if target and originalTransparency[target] then
			updateSelection(target)
		end
		return
	end
	
	if not Mouse.Target then return end

	if TeleportEnabled then
		local Location = Mouse.Hit.Position
		if not Location then return end
		GetRoot(Player).Velocity = Vector3.zero
		GetCharacter(Player):MoveTo(Location)
	elseif PhaseEnabled then
		local Target = Mouse.Target
		if not Target:IsA("BasePart") then return end
		SetObjectToPhase(Target)
	end
end)


-- UI Control

if not MainUI then return end

ToggleTween = nil

function Toggle()
	UIOpen = not UIOpen
	ContentShrunken.Visible = not UIOpen
	OuterFrame.Toggle.Text = UIOpen and "-" or "+"
	local Transparency = UIOpen and 0 or 1
	if ToggleTween then
		ToggleTween:Cancel()
	end
	Contents.Visible = UIOpen
	ToggleTween = TweenService:Create(OuterFrame, FadeTweenInfo, {BackgroundTransparency = Transparency})
	ToggleTween:Play()
end

OuterFrame.Toggle.MouseButton1Click:Connect(function()
	Toggle()
end)

OuterFrame.Info.MouseButton1Click:Connect(function()
	ShowInfo()
end)

OuterFrame.Info.MouseButton2Click:Connect(function()
	Toast("https://empty.tools/", 2)
end)

local function Close()
	getgenv().EmptyTools = false
	coroutine.resume(coroutine.create(function()
		saveSettings(userSettings)
		if LogsOpen then 
			if Log then
				Log:Destroy()
				LogsOpen = false
			end 
		end
		local PltInstance = workspace:FindFirstChild("PlatformFolder_" .. Player.UserId)
		if PltInstance then
			PltInstance:Destroy()
		end
		for _,PhaseObj in pairs(PhaseObjects) do
			if PhaseObj:FindFirstChild("PhaseHighlight") then
				PhaseObj.PhaseHighlight:Destroy()
				PhaseObj.CanCollide = true
			end
		end
		workspace.FallenPartsDestroyHeight = DefaultDestroyHeight
		SetGameShaders("None")
		SetCameraTarget("None", nil)
		SetPlayerAnimations("None")
		ToggleFlight(false)
		if Player.PlayerGui:FindFirstChild("LinkScreen") then
			Player.PlayerGui.LinkScreen:Destroy()
		end
		StopCFrameBoosting()
		TeleportEnabled = false
		if TeleportIndicator then
			TeleportIndicator:Destroy()
		end
		for i,v in pairs(Players:GetPlayers()) do
			coroutine.resume(coroutine.create(function()
				v:SetAttribute("Blocked", false)
				task.wait()
				TogglePlayerCharacter(v, true)
			end))
		end
		StopCarMode()
		StopOrbit()
		Camera.FieldOfView = DefaultFOV
		local Humanoid = GetHumanoid(Player) 
		if Humanoid then
			Humanoid.WalkSpeed = DefaultWalkSpeed
			Humanoid.JumpPower = DefaultJumpPower
			workspace.Gravity = DefaultGravity
		end
	end))
	wait(0.005)
	TeleportIndicator = nil
	PhaseEnabled = false
	MarkersActive = false
	MarkerUI:Destroy()
	MainUI:Destroy()
	coroutine.resume(coroutine.create(function()
		script:Destroy()
	end))
end

function MinimizeClose()
	OuterFrame.Visible = false
	MainUI:SetAttribute("Minimized", true)
	Toast('Press " ' .. UserInputService:GetStringForKeyCode(userSettings.ToggleKey) .. ' " to re-open Empty Tools', 1.5)
end

local confirmClose = false

OuterFrame.Close.MouseButton1Click:Connect(function()
	if MinimizeOnClose == true and not UserInputService.TouchEnabled then
		MinimizeClose()
	
		return
	end
	if UserInputService.TouchEnabled then
		if confirmClose then
			Close()
		else
			confirmClose = true
			OuterFrame.Close.Text = "?"
			coroutine.resume(coroutine.create(function()
				wait(1.5)
				confirmClose = false
				OuterFrame.Close.Text = "X"
			end))
		end
	else
		Close()
	end
end)

OuterFrame.Close.MouseButton2Click:Connect(function()
	Close()
end)

Contents.Category.MouseButton1Click:Connect(function()
	local Categories = CommandCategories
	local CategoryIndex = table.find(Categories, CurrentCategory)
	CategoryIndex = CategoryIndex + 1
	if CategoryIndex > #Categories then
		CategoryIndex = 1
	end
	CurrentCategory = Categories[CategoryIndex]

	Contents.Category.Text = "Filter: " .. CurrentCategory

	local SearchText = string.lower(Contents.Search.Text)
	for _, Command in pairs(CommandFrames) do
		local Category = Command:GetAttribute("Category")
		local CommandMatchesCategory = (CurrentCategory == "None" or Category == CurrentCategory)
		local CommandMatchesSearch = (SearchText == "" or #SearchText < 1 or string.find(string.lower(Command.Name), SearchText))

		Command.Visible = CommandMatchesCategory and CommandMatchesSearch
	end
	Contents.Commands.CanvasPosition = Vector2.zero
end)

Contents.Search:GetPropertyChangedSignal("Text"):Connect(function()
	local SearchText = string.lower(Contents.Search.Text)

	for _, Command in pairs(CommandFrames) do
		local Category = Command:GetAttribute("Category")
		local CommandMatchesCategory = (CurrentCategory == "None" or Category == CurrentCategory)
		local CommandMatchesSearch = (SearchText == "" or #SearchText < 1 or string.find(string.lower(Command.Name), SearchText))

		Command.Visible = CommandMatchesCategory and CommandMatchesSearch
	end

	Contents.Commands.CanvasPosition = Vector2.zero

	for _, Option in pairs(SettingsFrames) do
		local OptionTitleMatchesSearch = (SearchText == "" or #SearchText < 1 or string.find(string.lower(Option.Name), SearchText))
		local OptionInfoMatchesSearch = (SearchText == "" or #SearchText < 1 or string.find(string.lower(Option.Info.Text), SearchText))

		Option.Visible = OptionTitleMatchesSearch or OptionInfoMatchesSearch
	end

	Contents.SettingsFrame.CanvasPosition = Vector2.zero
end)

Contents.Settings.MouseButton1Click:Connect(function()
	if Contents.Commands.Visible then
		Contents.Settings.BackgroundColor3 = Color3.fromRGB(4, 4, 4)
		Contents.Commands.Visible = false
		Contents.SettingsFrame.Visible = true
	else
		Contents.Settings.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
		Contents.Commands.Visible = true
		Contents.SettingsFrame.Visible = false
	end
end)

-- Player Specific 

Players.LocalPlayer.CharacterAdded:Connect(function(character)

	local root = character:WaitForChild("HumanoidRootPart")
	if playerSpawnPoint then

		root.CFrame = playerSpawnPoint
	end
end)



local TeleportCheck = false
Players.LocalPlayer.OnTeleport:Connect(function(State)
	if (not TeleportCheck) and queueTeleport and Persist then
		TeleportCheck = true
		getgenv().EmptyTools = false
		if PlusEnabled then
			queueTeleport(EmptyToolsLoadstrings.Beta)
		else
			queueTeleport(EmptyToolsLoadstrings.Default)
		end
	end
end)

-- Input

local LocalJumpInstances = JumpInstances

function JumpReq()
	if not DoubleJump then return end
	local Root = GetRoot(Player)
	local Humanoid = GetHumanoid(Player)
	if Root and Humanoid then
		if Humanoid:GetState() == Enum.HumanoidStateType.Freefall then
			if LocalJumpInstances >= 1 then
				LocalJumpInstances -= 1
				Humanoid:ChangeState(Enum.HumanoidStateType.Jumping, true)
				Humanoid.StateChanged:Connect(function(old, new)
					if new == Enum.HumanoidStateType.Landed then
						LocalJumpInstances = JumpInstances
					end
				end)
			end
		end
	end
end

UserInputService.JumpRequest:Connect(function()
	JumpReq()
end)

UserInputService.InputBegan:Connect(function(Input, GPE) 
	if GPE then return end
	if KeySelected and KeyButton then
		KeySelected = false
		if Input.KeyCode == Enum.KeyCode.Unknown then
			return
		end
		userSettings.ToggleKey = Input.KeyCode.Name
		ToggleKey = Enum.KeyCode[userSettings.ToggleKey]
		saveSettings(userSettings)
		SetKeyMode(KeyButton, Input.KeyCode)
		KeyButton = nil
		return
	end
	if Input.KeyCode == Enum.KeyCode.Space then
		JumpReq()
	end
	if Input.KeyCode == ToggleKey then
		if MinimizeOnClose == true then
			if OuterFrame.Visible == true then
				MinimizeClose()
			else
				OuterFrame.Visible = true
				MainUI:SetAttribute("Minimized", true)
			end
		else
			Toggle()
		end
	end

	if modeEnabled and Input.KeyCode == Enum.KeyCode.Backspace then
		if currentSelection then
			local p = Instance.new("Part", game:GetService("ReplicatedFirst"))
			selectionBox.Parent = p
			if reparent then
				local OrigParent = Instance.new("ObjectValue", currentSelection)
				OrigParent.Name = "OG"
				OrigParent.Value = currentSelection.Parent
				currentSelection.Parent = reparentLocation
			else
				originalTransparency[currentSelection] = nil 
				currentSelection:Destroy()
			end
			selectionBox.Adornee = nil
			selectionBox.Visible = false
			currentSelection = nil
		end
	end
end)

-- Loop

RunService.RenderStepped:Connect(function()
	if TeleportEnabled and PointTeleportIndicatorEnabled then
		if TeleportIndicator then
			TeleportIndicator:MoveTo(Mouse.Hit.Position)
			local Part = TeleportIndicator:GetChildren()[1]
			Part.Position = TeleportIndicator.WorldPivot.Position
			local Root = GetRoot(Player)
			if Root then
				if (Root.Position - Part.Position).Magnitude < 7 then
					Part.CanCollide = false
				else
					Part.CanCollide = true
				end
			end
		else
			if PointTeleportIndicatorEnabled then
				TeleportIndicator = CreateTeleportIndicator()
			end
		end
	elseif TeleportIndicator ~= nil then
		TeleportIndicator:Destroy()
		TeleportIndicator = nil
	end

	if ReturnOnFall == true then
		local Root = GetRoot(Player)
		if Root then
			if Root.Position.Y < (DefaultDestroyHeight + 18) then
				local SpawnLocation = workspace:FindFirstChildOfClass("SpawnLocation")
				if SpawnLocation then
					Root.Velocity = Vector3.zero
					Root.CFrame = SpawnLocation.CFrame + Vector3.new(0,4.5,0)
				end
			end
		end
	end

	for i,v in pairs(Players:GetPlayers()) do
		if v:GetAttribute("Blocked") == true then
			local Character = GetCharacter(v)
			if Character:GetAttribute("CharacterHidden") == true then continue end
			Character:SetAttribute("CharacterHidden", true)
			TogglePlayerCharacter(v, false)
		else
			local Character = GetCharacter(v)
			if Character:GetAttribute("CharacterHidden") == true then
				Character:SetAttribute("CharacterHidden", false)
				TogglePlayerCharacter(v, true)
			end
		end
	end
	
	if Camera == nil then
		Camera = workspace.CurrentCamera
	end
end)
