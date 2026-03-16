# Mm2-yago-real
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

-- --- GUI ANA YAPI ---
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local espEnabled = false

-- 1. KÜÇÜK YAGO LOGOSU
local MenuButton = Instance.new("TextButton", ScreenGui)
MenuButton.Size = UDim2.new(0, 60, 0, 60)
MenuButton.Position = UDim2.new(0, 10, 0.4, 0)
MenuButton.BackgroundColor3 = Color3.fromRGB(40, 40, 150)
MenuButton.Text = "YAGO"
MenuButton.TextColor3 = Color3.new(1, 1, 1)
MenuButton.Font = Enum.Font.GothamBold
MenuButton.TextSize = 14
local LogoCorner = Instance.new("UICorner", MenuButton)
LogoCorner.CornerRadius = UDim.new(0, 30)

-- 2. ANA PANEL
local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0, 0, 0, 0)
Main.Position = UDim2.new(0.05, 0, 0.25, 0)
Main.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Main.ClipsDescendants = true
Main.Visible = false
Instance.new("UICorner", Main)

local Scroll = Instance.new("ScrollingFrame", Main)
Scroll.Size = UDim2.new(1, -10, 0.85, 0)
Scroll.Position = UDim2.new(0, 5, 0.12, 0)
Scroll.BackgroundTransparency = 1
Scroll.CanvasSize = UDim2.new(0, 0, 1.2, 0)
Instance.new("UIListLayout", Scroll).HorizontalAlignment = Enum.HorizontalAlignment.Center

-- AÇILIŞ ANİMASYONU
local isOpen = false
MenuButton.MouseButton1Click:Connect(function()
    if not isOpen then
        Main.Visible = true
        Main:TweenSize(UDim2.new(0, 220, 0, 350), "Out", "Quart", 0.4, true)
        isOpen = true
    else
        Main:TweenSize(UDim2.new(0, 0, 0, 0), "In", "Quart", 0.4, true, function() Main.Visible = false end)
        isOpen = false
    end
end)

-- BUTON OLUŞTURUCU
local function createBtn(text, callback)
    local btn = Instance.new("TextButton", Scroll)
    btn.Size = UDim2.new(0.9, 0, 0, 35)
    btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    btn.Text = text
    btn.TextColor3 = Color3.white
    btn.Font = Enum.Font.Gotham
    Instance.new("UICorner", btn)
    btn.MouseButton1Click:Connect(function() callback(btn) end)
end

-- --- ÖZELLİKLER ---

-- YAZILI ESP FONKSİYONU
local function createTag(char, text, color)
    if char:FindFirstChild("YagoTag") then char.YagoTag:Destroy() end
    local head = char:WaitForChild("Head")
    local billing = Instance.new("BillboardGui", char)
    billing.Name = "YagoTag"
    billing.Adornee = head
    billing.Size = UDim2.new(0, 100, 0, 50)
    billing.StudsOffset = Vector3.new(0, 3, 0)
    billing.AlwaysOnTop = true
    local label = Instance.new("TextLabel", billing)
    label.BackgroundTransparency = 1
    label.Size = UDim2.new(1, 0, 1, 0)
    label.Text = text
    label.TextColor3 = color
    label.Font = Enum.Font.GothamBold
    label.TextSize = 18
    label.TextStrokeTransparency = 0
end

createBtn("Yazılı ESP: KAPALI", function(btn)
    espEnabled = not espEnabled
    btn.Text = espEnabled and "Yazılı ESP: AÇIK" or "Yazılı ESP: KAPALI"
    btn.BackgroundColor3 = espEnabled and Color3.fromRGB(0, 120, 0) or Color3.fromRGB(45, 45, 45)
end)

createBtn("Yago Hız (70)", function() Player.Character.Humanoid.WalkSpeed = 70 end)

-- DÖNGÜ (ESP)
RunService.RenderStepped:Connect(function()
    if espEnabled then
        for _, v in pairs(Players:GetPlayers()) do
            if v ~= Player and v.Character and v.Character:FindFirstChild("Head") then
                local isM = v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")
                local isS = v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun")
                
                if isM then
                    createTag(v.Character, "[!] KATİL", Color3.new(1, 0, 0))
                elseif isS then
                    createTag(v.Character, "[?] ŞERİF", Color3.new(0, 0, 1))
                else
                    createTag(v.Character, "MASUM", Color3.new(1, 1, 1))
                end
            end
        end
    else
        for _, v in pairs(Players:GetPlayers()) do
            if v.Character and v.Character:FindFirstChild("YagoTag") then v.Character.YagoTag:Destroy() end
        end
    end
end)

-- Giriş Bildirimi
game.StarterGui:SetCore("SendNotification", {
    Title = "YAGO HUB",
    Text = "Yago Hub v5 Yüklendi! 🦅",
    Duration = 5
})
