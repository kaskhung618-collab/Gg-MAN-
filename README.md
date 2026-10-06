# Gg-MAN-
-- =======================================================
-- ⚡ สคริปต์ INFINITY HAX (VOID UI - READY TO USE VERSION)
-- =======================================================

-- [1] ระบบ BYPASS ลบหน้าต่างคีย์ / KEEP ของ CUPIEHUB เบื้องหลัง
task.spawn(function()
    local CoreGui = game:GetService("CoreGui")
    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer
    local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

    while task.wait(0.2) do
        pcall(function()
            for _, ui in ipairs(CoreGui:GetChildren()) do
                if string.find(string.lower(ui.Name), "cupie") or string.find(string.lower(ui.Name), "keep") or ui:FindFirstChild("KeyFrame") then
                    ui:Destroy()
                end
            end
            for _, ui in ipairs(PlayerGui:GetChildren()) do
                if string.find(string.lower(ui.Name), "cupie") or string.find(string.lower(ui.Name), "keep") or ui:FindFirstChild("KeyFrame") then
                    ui:Destroy()
                end
            end
        end)
    end
end)

-- โหลดตัวสคริปต์หลักต้นทาง
pcall(function()
    loadstring(game:HttpGet("https://cupiehub.xyz"))()
end)


-- [2] เริ่มต้นโหลดหน้าต่างเมนูด้วย VOID UI LIBRARY
local library = loadstring(game:HttpGet("https://githubusercontent.com"))()

local Window = library:Load({
    name = "INFINITY HAX V2",
    sizex = 450,
    sizey = 400,
    theme = "Dark"
})

-- สร้างแท็บและเซกชันตามไวยากรณ์โครงสร้างหลักของ VoidUi
local MainTab = Window:Tab("Main Features")
local LeftSection = MainTab:Section({ name = "Hitbox Options", side = "left" })
local RightSection = MainTab:Section({ name = "Player Options", side = "right" })

-- ตัวแปรส่วนกลางสำหรับลูปเช็กค่าทำงาน (Core Variables)
local HitboxActive = false
local InfiniteJumpActive = false
_G.HitboxSize = 20
_G.HitboxTransparency = 0.65

-- [3] CORE LOGIC: ระบบควบคุมการทำงาน (Real-time Loop)
task.spawn(function()
    while true do
        task.wait(0.2)
        if HitboxActive then
            pcall(function()
                for _, player in ipairs(game.Players:GetPlayers()) do
                    if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
                        local head = player.Character.Head
                        if head.Size.X ~= _G.HitboxSize or head.Transparency ~= _G.HitboxTransparency then
                            head.Size = Vector3.new(_G.HitboxSize, _G.HitboxSize, _G.HitboxSize)
                            head.Transparency = _G.HitboxTransparency
                            head.BrickColor = BrickColor.new("Really red")
                            head.Material = Enum.Material.Neon
                            head.CanCollide = false
                        end
                    end
                end
            end)
        end
    end
end)

-- ฟังก์ชันคืนค่าเริ่มต้นให้ผู้เล่นอื่นเมื่อทำการปิดระบบขยาย Hitbox
local function resetHitboxes()
    pcall(function()
        for _, player in ipairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
                local head = player.Character.Head
                head.Size = Vector3.new(2, 1, 1)
                head.Transparency = 0
                head.BrickColor = BrickColor.new("Medium stone grey")
                head.Material = Enum.Material.Plastic
                head.CanCollide = true
            end
        end
    end)
end

-- ระบบดักจับคำสั่งการกระโดดไม่จำกัด (Infinite Jump)
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = game.Players.LocalPlayer

UserInputService.JumpRequest:Connect(function()
    if InfiniteJumpActive then
        pcall(function()
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
                LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end)
    end
end)


-- [4] UI ELEMENTS: การผูกปุ่มกับสคริปต์ควบคุม (เชื่อมโยงผ่านวัตถุ Section)

-- --- คอลัมน์ฝั่งซ้าย: ตั้งค่าเป้าและขนาดกล่องศัตรู ---
LeftSection:Toggle({
    name = "Enable Hitbox Expander",
    default = false,
    flag = "hitbox_toggle_on",
    callback = function(Value)
        HitboxActive = Value
        if not HitboxActive then
            resetHitboxes()
        end
    end
})

LeftSection:Slider({
    name = "Hitbox Size",
    min = 2,
    max = 100,
    default = 20,
    float = 1,
    text = "Size: [value]",
    flag = "hitbox_size_val",
    callback = function(Value)
        _G.HitboxSize = Value
    end
})

LeftSection:Slider({
    name = "Hitbox Transparency",
    min = 0,
    max = 100,
    default = 65,
    float = 1,
    text = "Visibility: [value]%",
    flag = "hitbox_trans_val",
    callback = function(Value)
        _G.HitboxTransparency = Value / 100
    end
})

-- --- คอลัมน์ฝั่งขวา: ตั้งค่าสถานะและการขยับของตัวละครเรา ---
RightSection:Toggle({
    name = "Infinite Jump",
    default = false,
    flag = "inf_jump_on",
    callback = function(Value)
        InfiniteJumpActive = Value
    end
})

RightSection:Slider({
    name = "WalkSpeed (ความเร็ว)",
    min = 16,
    max = 200,
    default = 16,
    float = 1,
    text = "Speed: [value]",
    flag = "player_speed_val",
    callback = function(Value)
        pcall(function()
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
                LocalPlayer.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = Value
            end
        end)
    end
})

RightSection:Slider({
    name = "JumpPower (แรงโดด)",
    min = 50,
    max = 300,
    default = 50,
    float = 1,
    text = "Power: [value]",
    flag = "player_jump_val",
    callback = function(Value)
        pcall(function()
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
                local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
                humanoid.UseJumpPower = true
                humanoid.JumpPower = Value
            end
        end)
    end
})

-- ลูปคอยตรวจเช็กเผื่อเกมทำการบังคับรีเซ็ตสถานะความเร็วเมื่อเกิดใหม่
task.spawn(function()
    while task.wait(0.5) do
        pcall(function()
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
                local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
                if library.flags["player_speed_val"] and library.flags["player_speed_val"] ~= 16 then
                    humanoid.WalkSpeed = library.flags["player_speed_val"]
                end
                if library.flags["player_jump_val"] and library.flags["player_jump_val"] ~= 50 then
                    humanoid.UseJumpPower = true
                    humanoid.JumpPower = library.flags["player_jump_val"]
                end
            end
        end)
    end
end)

-- ส่งการแจ้งเตือนของ Roblox ขึ้นที่มุมขวาเมื่อทุกอย่างติดตั้งพร้อมกดใช้งาน
game:GetService("StarterGui"):SetCore("SendNotification", {
    Title = "VoidUi Inject Success",
    Text = "เมนูเปิดเสร็จสมบูรณ์ สามารถปรับแต่งค่าได้ทันทีครับ!",
    Duration = 5
})
