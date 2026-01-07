--====================================================
-- RASY11D HUB
-- Version : 1.0
-- UI      : Orion UI
-- Platform: Mobile Only
-- Feature : ESP Team Color + Skeleton
-- Author  : RASY11D
--====================================================

--================ LOAD CHECK ========================
if not game:IsLoaded() then
	game.Loaded:Wait()
end

--================ MOBILE CHECK ======================
local UIS = game:GetService("UserInputService")
if not UIS.TouchEnabled then
	warn("[RASY11D HUB] Mobile only")
	return
end

--================ ORION UI ==========================
local OrionLib
local success, err = pcall(function()
	OrionLib = loadstring(game:HttpGet(
		"https://raw.githubusercontent.com/jensonhirst/Orion/main/source"
	))()
end)

if not success or not OrionLib then
	warn("[RASY11D HUB] Failed to load Orion UI")
	return
end

--================ SERVICES ==========================
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

--================ GUI PARENT ========================
local GuiParent = gethui and gethui() or game.CoreGui

--================ STATE =============================
local ESP_ENABLED = true
local Skeletons = {}
local DrawingSupported = Drawing ~= nil

--====================================================
-- INTRO ANIMATION
--====================================================
pcall(function()
	local IntroGui = Instance.new("ScreenGui")
	IntroGui.Name = "RASY11D_INTRO"
	IntroGui.Parent = GuiParent

	local Frame = Instance.new("Frame", IntroGui)
	Frame.Size = UDim2.new(1,0,1,0)
	Frame.BackgroundColor3 = Color3.new(0,0,0)
	Frame.BackgroundTransparency = 1

	local Text = Instance.new("TextLabel", Frame)
	Text.Size = UDim2.new(1,0,1,0)
	Text.Text = "RASY11D HUB"
	Text.Font = Enum.Font.GothamBlack
	Text.TextSize = 42
	Text.TextColor3 = Color3.new(1,1,1)
	Text.BackgroundTransparency = 1
	Text.TextTransparency = 1

	TweenService:Create(Frame, TweenInfo.new(0.5),
		{BackgroundTransparency = 0}):Play()
	task.wait(0.2)
	TweenService:Create(Text, TweenInfo.new(0.6),
		{TextTransparency = 0}):Play()
	task.wait(1.2)
	TweenService:Create(Text, TweenInfo.new(0.5),
		{TextTransparency = 1}):Play()
	TweenService:Create(Frame, TweenInfo.new(0.5),
		{BackgroundTransparency = 1}):Play()

	task.wait(0.6)
	IntroGui:Destroy()
end)

--====================================================
-- ORION WINDOW
--====================================================
local Window = OrionLib:MakeWindow({
	Name = "RASY11D HUB | MOBILE",
	HidePremium = true,
	SaveConfig = false,
	IntroEnabled = false
})

local Tab = Window:MakeTab({
	Name = "ESP",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

Tab:AddLabel("🖤 RASY11D HUB")
Tab:AddLabel("🎨 ESP : Team Color")
Tab:AddLabel("🦴 Skeleton : "..(DrawingSupported and "Enabled" or "Not Supported"))
Tab:AddLabel("📱 Mobile Optimized")

--====================================================
-- HIGHLIGHT ESP
--====================================================
local function applyHighlight(player, character)
	if not character then return end

	local hl = character:FindFirstChild("RASY11D_HL")
	if hl then
		hl.Enabled = ESP_ENABLED
		return
	end

	hl = Instance.new("Highlight")
	hl.Name = "RASY11D_HL"
	hl.FillTransparency = 0.55
	hl.OutlineTransparency = 0
	hl.FillColor = player.TeamColor.Color
	hl.OutlineColor = Color3.new(1,
