function love.load()
    player = {
        x = 400,
        y = 300,
        speed = 600,
        angle = 0,
        bullets = {},
        fireRate = 0.1,
        fireTimer = 0,
        sensitivity = 10,
        autoAim = true
    }

    -- أهداف متحركة للتصويب عليها
    targets = {
        {x = 100, y = 100},
        {x = 700, y = 150},
        {x = 300, y = 500},
    }
end

-- دالة لحساب المسافة بين نقطتين
local function distance(x1, y1, x2, y2)
    return ((x2 - x1)^2 + (y2 - y1)^2)^0.5
end

function love.update(dt)
    -- تحريك الأهداف بشكل بسيط (تتحرك يمين ويسار)
    for i, t in ipairs(targets) do
        t.x = t.x + math.sin(love.timer.getTime() + i) * 20 * dt
    end

    -- حركة اللاعب
    local moveX, moveY = 0, 0
    if love.keyboard.isDown("w") then moveY = moveY - 1 end
    if love.keyboard.isDown("s") then moveY = moveY + 1 end
    if love.keyboard.isDown("a") then moveX = moveX - 1 end
    if love.keyboard.isDown("d") then moveX = moveX + 1 end

    local length = math.sqrt(moveX * moveX + moveY * moveY)
    if length > 0 then
        moveX = moveX / length
        moveY = moveY / length
    end

    player.x = player.x + moveX * player.speed * dt
    player.y = player.y + moveY * player.speed * dt

    -- تتبع الرأس مع تصويب تلقائي
    local targetAngle
    if player.autoAim and #targets > 0 then
        -- إيجاد أقرب هدف
        local closestTarget = nil
        local closestDist = math.huge
        for _, t in ipairs(targets) do
            local dist = distance(player.x, player.y, t.x, t.y)
            if dist < closestDist then
                closestDist = dist
                closestTarget = t
            end
        end
        targetAngle = math.atan2(closestTarget.y - player.y, closestTarget.x - player.x)
    else
        -- تتبع الماوس إذا التصويب التلقائي مغلق
        local mouseX, mouseY = love.mouse.getPosition()
        targetAngle = math.atan2(mouseY - player.y, mouseX - player.x)
    end

    local diff = targetAngle - player.angle
    if diff > math.pi then diff = diff - 2 * math.pi
    elseif diff < -math.pi then diff = diff + 2 * math.pi end

    player.angle = player.angle + diff * dt * player.sensitivity

    -- إطلاق النار تلقائياً
    player.fireTimer = player.fireTimer - dt
    if player.fireTimer <= 0 then
        fireBullet(player.x, player.y, player.angle)
        player.fireTimer = player.fireRate
    end

    -- تحديث الطلقات وحذفها خارج الشاشة
    for i = #player.bullets, 1, -1 do
        local b = player.bullets[i]
        b.x = b.x + math.cos(b.angle) * b.speed * dt
        b.y = b.y + math.sin(b.angle) * b.speed * dt

        if b.x < 0 or b.x > love.graphics.getWidth() or b.y < 0 or b.y > love.graphics.getHeight() then
            table.remove(player.bullets, i)
        end
    end
end

function fireBullet(x, y, angle)
    local bullet = {
        x = x,
        y = y,
        angle = angle,
        speed = 500,
        size = 10
    }
    table.insert(player.bullets, bullet)
end

function love.draw()
    -- ما في رسم للاعب والطلقات حسب طلبك، بس نرسم الأهداف عشان تعرف وين تصوب
    love.graphics.setColor(0, 1, 0) -- أخضر للأهداف
    for _, t in ipairs(targets) do
        love.graphics.circle("fill", t.x, t.y, 15)
    end
end
