import base64

# Lua script content (placeholder, actual script would be inserted here)
lua_script = """
-- Nepo's Chill Hub v3.0 (ESP, Teleport, Invincibility, GUI Toggle)

local player = game.Players.LocalPlayer
local uis = game:GetService("UserInputService")
local runService = game:GetService("RunService")
local camera = workspace.CurrentCamera

local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "ChillHubGui"

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 300, 0, 260)
Frame.Position = UDim2.new(0.5, -150, 0, 100)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.Visible = true

local Title = Instance.new("TextLabel", Frame)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "Nepo's Chill Hub"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 20

-- ESP toggle
local ESPEnabled = false
local function createESP()
    for _, p in ipairs(game.Players:GetPlayers()) do
        if p ~= player and p.Character and p.Character:FindFirstChild("Head") then
            if not p.Character:FindFirstChild("ESP") then
                local esp = Instance.new("BillboardGui", p.Character.Head)
                esp.Name = "ESP"
                esp.Size = UDim2.new(0, 100, 0, 40)
                esp.StudsOffset = Vector3.new(0, 3, 0)
                esp.AlwaysOnTop = true

                local label = Instance.new("TextLabel", esp)
                label.Size = UDim2.new(1, 0, 1, 0)
                label.Text = p.Name
                label.BackgroundTransparency = 1
                label.TextColor3 = Color3.new(1, 0, 0)
                label.Font = Enum.Font.GothamBold
                label.TextSize = 14
            end
        end
    end
end

-- Teleport to random player
local function teleportToRandom()
    local players = game.Players:GetPlayers()
    for _, p in ipairs(players) do
        if p ~= player and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                hrp.CFrame = p.Character.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0)
            end
            break
        end
    end
end

-- Invincibility (anchor humanoid root)
local function becomeInvincible()
    local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if hrp then hrp.Anchored = true end
end

-- GUI Toggle (F key)
uis.InputBegan:Connect(function(input, gp)
    if not gp and input.KeyCode == Enum.KeyCode.F then
        Frame.Visible = not Frame.Visible
    end
end)

-- Buttons
local function makeButton(name, yPos, callback)
    local btn = Instance.new("TextButton", Frame)
    btn.Size = UDim2.new(0.8, 0, 0, 30)
    btn.Position = UDim2.new(0.1, 0, 0, yPos)
    btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 14
    btn.Text = name
    btn.MouseButton1Click:Connect(callback)
end

makeButton("Enable ESP", 50, function() ESPEnabled = true end)
makeButton("Teleport to Player", 90, teleportToRandom)
makeButton("Invincibility", 130, becomeInvincible)

-- ESP Loop
runService.RenderStepped:Connect(function()
    if ESPEnabled then
        createESP()
    end
end)
"""

# Encode the script to base64
encoded_script = base64.b64encode(lua_script.encode()).decode()

# Generate loadstring format
loadstring_script = f'loadstring(game:HttpGet("https://pastebin.com/raw/REPLACEME"))()'

encoded_script[:300]  # Show a snippet of the encoded script (for verification)