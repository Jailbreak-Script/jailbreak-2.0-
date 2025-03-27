-- // ЗАГРУЗКА БИБЛИОТЕКИ GUI
local Rayfield = loadstring(game:HttpGet('https://raw.githubusercontent.com/shlexware/Rayfield/main/source'))()

-- // СОЗДАНИЕ ОКНА
local Window = Rayfield:CreateWindow({
   Name = "Jailbreak Cheat",
   LoadingTitle = "Jailbreak Hack",
   LoadingSubtitle = "by Jailbreak-Script",
   ConfigurationSaving = {Enabled = false},
   KeySystem = false
})

-- // СОЗДАНИЕ ВКЛАДКИ
local MainTab = Window:CreateTab("Главная")

-- // ФУНКЦИЯ ТЕЛЕПОРТАЦИИ
local function teleportTo(position)
   local player = game.Players.LocalPlayer
   if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
      player.Character.HumanoidRootPart.CFrame = CFrame.new(position)
   end
end

-- // КНОПКА АВТО-ОГРАБЛЕНИЯ
local AutoRobEnabled = false
local function startAutoRob()
   while AutoRobEnabled do
      for _, location in pairs(robLocations) do
         if location.check() then
            teleportTo(location.position)
            wait(3)
            location.interact()
            wait(2)
            teleportTo(location.cashout)
            wait(3)
            game:GetService("ReplicatedStorage"):FindFirstChild("CashIn"):FireServer()
         end
      end
      wait(5)
   end
end

MainTab:CreateToggle({
   Name = "Авто-Ограбление",
   CurrentValue = false,
   Callback = function(value)
      AutoRobEnabled = value
      if value then
         startAutoRob()
      end
   end
})

-- // КНОПКА АВТО-АРЕСТА
local AutoArrestEnabled = false
local function startAutoArrest()
   while AutoArrestEnabled do
      for _, player in pairs(game.Players:GetPlayers()) do
         if player ~= game.Players.LocalPlayer then
            game:GetService("ReplicatedStorage"):FindFirstChild("Arrest"):FireServer(player)
         end
      end
      wait(5)
   end
end

MainTab:CreateToggle({
   Name = "Авто-Арест",
   CurrentValue = false,
   Callback = function(value)
      AutoArrestEnabled = value
      if value then
         startAutoArrest()
      end
   end
})

-- // КНОПКА ТЕЛЕПОРТАЦИИ
MainTab:CreateInput({
   Name = "Телепорт к игроку",
   PlaceholderText = "Введите имя игрока",
   Callback = function(text)
      local target = game.Players:FindFirstChild(text)
      if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
         teleportTo(target.Character.HumanoidRootPart.Position)
      end
   end
})

-- // КНОПКА ВЫДАЧИ ДЕНЕГ
MainTab:CreateButton({
   Name = "Выдать деньги",
   Callback = function()
      game:GetService("ReplicatedStorage"):FindFirstChild("GiveMoney"):FireServer(10000) -- Меняй сумму тут
   end
})

-- // СКРИПТ DYNAMIC
-- Загрузим функционал `jailbreak_auto_rob.lua` внутри основного скрипта

local robLocations = {
   -- Пример локации для авто-ограбления
   {check = function() return true end, position = Vector3.new(0, 0, 0), interact = function() print("Interacting with rob location") end, cashout = Vector3.new(10, 0, 10)},
   -- Можно добавить больше локаций, если нужно
}

-- Авто-ограбление
local AutoRobEnabled = false
local function startAutoRob()
   while AutoRobEnabled do
      for _, location in pairs(robLocations) do
         if location.check() then
            teleportTo(location.position)
            wait(3)
            location.interact()
            wait(2)
            teleportTo(location.cashout)
            wait(3)
            game:GetService("ReplicatedStorage"):FindFirstChild("CashIn"):FireServer()
         end
      end
      wait(5)
   end
end

-- Механизм авто-ареста
local AutoArrestEnabled = false
local function startAutoArrest()
   while AutoArrestEnabled do
      for _, player in pairs(game.Players:GetPlayers()) do
         if player ~= game.Players.LocalPlayer then
            game:GetService("ReplicatedStorage"):FindFirstChild("Arrest"):FireServer(player)
         end
      end
      wait(5)
   end
end

-- Включение/выключение авто-ограбления
MainTab:CreateToggle({
   Name = "Авто-Ограбление",
   CurrentValue = false,
   Callback = function(value)
      AutoRobEnabled = value
      if value then
         startAutoRob()
      end
   end
})

-- Включение/выключение авто-ареста
MainTab:CreateToggle({
   Name = "Авто-Арест",
   CurrentValue = false,
   Callback = function(value)
      AutoArrestEnabled = value
      if value then
         startAutoArrest()
      end
   end
})

-- Телепорт к игроку
MainTab:CreateInput({
   Name = "Телепорт к игроку",
   PlaceholderText = "Введите имя игрока",
   Callback = function(text)
      local target = game.Players:FindFirstChild(text)
      if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
         teleportTo(target.Character.HumanoidRootPart.Position)
      end
   end
})

-- Кнопка выдачи денег
MainTab:CreateButton({
   Name = "Выдать деньги",
   Callback = function()
      game:GetService("ReplicatedStorage"):FindFirstChild("GiveMoney"):FireServer(10000) -- Меняй сумму тут
   end
})

