local DiscordLib =
    loadstring(game:HttpGet "https://raw.githubusercontent.com/bloodball/-back-ups-for-libs/main/discord")()

-- ตัวแปร shared สำหรับหยุด loop ข้าม UI
shared.autoFarmFlags = shared.autoFarmFlags or {
    AutoFish = false,
    AutoGold = false
}

local win = DiscordLib:Window("discord library")
local serv = win:Server("Preview", "")
local btns = serv:Channel("Buttons")
local tgls = serv:Channel("Toggles")

-- ปุ่ม: ขายปลา
btns:Button(
    "ขายปลา",
    function()
        local args = {
            [1] = "fish",
            [2] = false,
            [3] = false
        }
        game:GetService("ReplicatedStorage")
            :WaitForChild("Packages"):WaitForChild("Knit"):WaitForChild("Services")
            :WaitForChild("ShopService"):WaitForChild("RF"):WaitForChild("Shop")
            :InvokeServer(unpack(args))

        DiscordLib:Notification("ขายปลา", "ขายปลา 1 ครั้งเรียบร้อย", "โอเค")
    end
)

-- ปุ่ม: ขายทอง
btns:Button(
    "ขายทอง 1 ครั้ง",
    function()
        local args = {
            [1] = "gold",
            [2] = false,
            [3] = false
        }
        game:GetService("ReplicatedStorage")
            :WaitForChild("Packages"):WaitForChild("Knit"):WaitForChild("Services")
            :WaitForChild("ShopService"):WaitForChild("RF"):WaitForChild("Shop")
            :InvokeServer(unpack(args))

        DiscordLib:Notification("ขายทอง", "ขายทอง 1 ครั้งเรียบร้อย", "โอเค")
    end
)

-- Toggle: Auto-Fish
tgls:Toggle(
    "Auto-Fish",
    shared.autoFarmFlags.AutoFish,
    function(state)
        shared.autoFarmFlags.AutoFish = state
        if state then
            DiscordLib:Notification("Auto-Fish", "เริ่มฟาร์มปลา", "โอเค")
            spawn(function()
                while shared.autoFarmFlags.AutoFish do
                    local args1 = {
                        [1] = "fish",
                        [2] = false,
                        [3] = false
                    }
                    game:GetService("ReplicatedStorage")
                        :WaitForChild("Packages"):WaitForChild("Knit"):WaitForChild("Services")
                        :WaitForChild("ShopService"):WaitForChild("RF"):WaitForChild("Shop")
                        :InvokeServer(unpack(args1))

                    local args2 = {
                        [1] = "fish",
                        [2] = false,
                        [3] = true
                    }
                    game:GetService("ReplicatedStorage")
                        :WaitForChild("Packages"):WaitForChild("Knit"):WaitForChild("Services")
                        :WaitForChild("ShopService"):WaitForChild("RF"):WaitForChild("Shop")
                        :InvokeServer(unpack(args2))

                    wait(10)
                end
            end)
        else
            DiscordLib:Notification("Auto-Fish", "หยุดฟาร์มปลา", "โอเค")
        end
    end
)

-- Toggle: Auto-Gold
tgls:Toggle(
    "Auto-Gold",
    shared.autoFarmFlags.AutoGold,
    function(state)
        shared.autoFarmFlags.AutoGold = state
        if state then
            DiscordLib:Notification("Auto-Gold", "เริ่มฟาร์มทอง", "โอเค")
            spawn(function()
                while shared.autoFarmFlags.AutoGold do
                    local args = {
                        [1] = "gold",
                        [2] = false,
                        [3] = true
                    }
                    game:GetService("ReplicatedStorage")
                        :WaitForChild("Packages"):WaitForChild("Knit"):WaitForChild("Services")
                        :WaitForChild("ShopService"):WaitForChild("RF"):WaitForChild("Shop")
                        :InvokeServer(unpack(args))

                    wait(10)
                end
            end)
        else
            DiscordLib:Notification("Auto-Gold", "หยุดฟาร์มทอง", "โอเค")
        end
    end
)

-- ปุ่มปิด UI (RightControl) = หยุด auto ทุกอย่าง
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if input.KeyCode == Enum.KeyCode.RightControl then
        shared.autoFarmFlags.AutoFish = false
        shared.autoFarmFlags.AutoGold = false
        DiscordLib:Notification("ปิด UI", "หยุด Auto-Farm ทั้งหมด", "โอเค")
    end
end)

win:Server("Main", "http://www.roblox.com/asset/?id=6031075938")
