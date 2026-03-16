-- YAGO HUB v6 - KESİN ÇÖZÜM
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- PANEL OLUŞTURMA
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0, 200, 0, 250)
Main.Position = UDim2.new(0.1, 0, 0.4, 0)
Main.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Main.Active = true
Main.Draggable = true
Instance.new("UICorner", Main)

local Title = Instance.new("TextLabel", Main)
Title.Text = "YAGO HUB v6 🦅"
Title.Size = UDim2.new(1, 0, 0, 30)
Title.TextColor3 = Color3.white
Title.BackgroundColor3 = Color3.fromRGB(50, 50, 150)
Instance.new("UICorner", Title)

local espBtn = Instance.new("TextButton", Main)
espBtn.Text = "ESP: KAPALI"
espBtn.Size = UDim2.new(0.8, 0, 0, 40)
espBtn.Position = UDim2.new(0.1, 0, 0.2, 0)
espBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
espBtn.TextColor3 = Color3.white
Instance.new("UICorner", espBtn)

local speedBtn = Instance.new("TextButton", Main)
speedBtn.Text = "HIZ: 70"
speedBtn.Size = UDim2.new(0.8, 0, 0, 40)
speedBtn.Position = UDim2.new(0.1, 0, 0.5, 0)
speedBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
speedBtn.TextColor3 = Color3.white
Instance.new("UICorner", speedBtn)

-- ÖZELLİKLER
local espActive = false
espBtn.MouseButton1Click:Connect(function()
    espActive = not espActive
    espBtn.Text = espActive and "ESP: AÇIK" or "ESP: KAPALI"
    espBtn.BackgroundColor3 = espActive and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(60, 60, 60)
end)

speedBtn.MouseButton1Click:Connect(function()
    if Player.Character and Player.Character:FindFirstChild("Humanoid") then
        Player.Character.Humanoid.WalkSpeed = 70
    end
end)

-- ESP SİSTEMİ
RunService.RenderStepped:Connect(function()
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= Player and v.Character and v.Character:FindFirstChild("Head") then
            local isM = v.Backpack:FindFirstChild("Knife") or v.Character:FindFirstChild("Knife")
            local isS = v.Backpack:FindFirstChild("Gun") or v.Character:FindFirstChild("Gun")
            local tag = v.Character:FindFirstChild("YagoTag")
            
            if espActive then
                if not tag then
                    tag = Instance.new("BillboardGui", v.Character)
                    tag.Name = "YagoTag"
                    tag.Size = UDim2.new(0, 100, 0, 50)
                    tag.StudsOffset = Vector3.new(0, 3, 0)
                    tag.AlwaysOnTop = true
                    local l = Instance.new("TextLabel", tag)
                    l.Size = UDim2.new(1, 0, 1, 0)
                    l.BackgroundTransparency = 1
                    l.TextSize = 18
                    l.Font = Enum.Font.SourceSansBold
                end
                if isM then
                    tag.TextLabel.Text = "[!] KATİL"
                    tag.TextLabel.TextColor3 = Color3.new(1, 0, 0)
                elseif isS then
                    tag.TextLabel.Text = "[!] ŞERİF"
                    tag.TextLabel.TextColor3 = Color3.new(0, 0, 1)
                else
                    tag.TextLabel.Text = "MASUM"
                    tag.TextLabel.TextColor3 = Color3.new(1, 1, 1)
                end
            else
                if tag then tag:Destroy() end
            end
        end
    end
end)
