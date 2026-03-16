local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- --- PANEL OLUŞTURMA ---
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0, 200, 0, 250)
Main.Position = UDim2.new(0.1, 0, 0.4, 0)
Main.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Main.Active = true
Main.Draggable = true -- Paneli ekranda sürükleyebilirsin
Instance.new("UICorner", Main)

local Title = Instance.new("TextLabel", Main)
Title.Text = "YAGO HUB v6"
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
speedBtn.Position = UDim2.new(0.1, 0, 0.45, 0)
speedBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
speedBtn.TextColor3 = Color3.white
Instance.new("UICorner", speedBtn)

-- --- ÖZELLİKLER ---
local espActive = false
espBtn.MouseButton1Click:Connect(function()
    espActive = not espActive
    espBtn.Text = espActive and "ESP: AÇIK" or "ESP: KAPALI"
    espBtn.BackgroundColor3 = espActive and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(60, 60, 60)
end)

speedBtn.MouseButton1Click:Connect(function()
    Player.Character.Humanoid.WalkSpeed = 70
end)

-- --- ESP DÖNGÜSÜ (EN SAĞLAM HALİ) ---
RunService.RenderStepped:Connect(function()
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= Player and v.Character and v.Character:FindFirstChild("Head") then
            local char = v.Character
            -- Eşyaları kontrol et
            local isM = char:FindFirstChild("Knife") or (v.Backpack and v.Backpack:FindFirstChild("Knife"))
            local isS = char:FindFirstChild("Gun") or (v.Backpack and v.Backpack:FindFirstChild("Gun"))
            
            local tag = char:FindFirstChild("YagoTag")
            if espActive then
                if not tag then
                    tag = Instance.new("BillboardGui", char)
                    tag.Name = "YagoTag"
                    tag.Size = UDim2.new(0, 100, 0, 50)
                    tag.StudsOffset = Vector3.new(0, 3, 0)
                    tag.AlwaysOnTop = true
                    local l = Instance.new("TextLabel", tag)
                    l.Size = UDim2.new(1, 0, 1, 0)
                    l.BackgroundTransparency = 1
                    l.TextSize = 16
                    l.Font = Enum.Font.GothamBold
                end
                
                local label = tag.TextLabel
                if isM then
                    label.Text = "[!] KATİL"
                    label.TextColor3 = Color3.new(1, 0, 0)
                elseif isS then
                    label.Text = "[!] ŞERİF"
                    label.TextColor3 = Color3.new(0, 0, 1)
                else
                    label.Text = "MASUM"
                    label.TextColor3 = Color3.new(1, 1, 1)
                end
            else
                if tag then tag:Destroy() end
            end
        end
    end
end)
