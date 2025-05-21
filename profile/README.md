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

    targets = {
        {x = 200, y = 200, radius = 20, isAlive = true},
        {x = 600, y = 300, radius = 20, isAlive = true},
    }
end

local function distance(x1, y1, x2, y2)
    return ((x2 - x1)^2 + (y2 - y1)^2)^0.5
end

function love.update(dt)
    for i, t in ipairs(targets) do
        t.x = t.x + math.sin(love.timer.getTime() + i) * 20 * dt
    end

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

    local targetAngle
    local closestTarget = nil
    local closestDist = math.huge

    for _, t in ipairs(targets) do
        if t.isAlive then
            local dist = distance(player.x, player.y, t.x, t.y)
            if dist < closestDist then
                closestDist = dist
                closestTarget = t
            end
        end
    end

    if player.autoAim and closestTarget then
        targetAngle = math.atan2(closestTarget.y - player.y, closestTarget.x - player.x)
    else
        local mx, my = love.mouse.getPosition()
        targetAngle = math.atan2(my - player.y, mx - player.x)
    end

    local diff = targetAngle - player.angle
    if diff > math.pi then diff = diff - 2 * math.pi
    elseif diff < -math.pi then diff = diff + 2 * math.pi end

    player.angle = player.angle + diff * dt * player.sensitivity

    player.fireTimer = player.fireTimer - dt
    if player.fireTimer <= 0 and closestTarget then
        fireBullet(player.x, player.y, player.angle)
        player.fireTimer = player.fireRate
    end

    for i = #player.bullets, 1, -1 do
        local b = player.bullets[i]
        b.x = b.x + math.cos(b.angle) * b.speed * dt
        b.y = b.y + math.sin(b.angle) * b.speed * dt

        for _, t in ipairs(targets) do
            if t.isAlive then
                local bodyDist = distance(b.x, b.y, t.x, t.y)
                if bodyDist < t.radius then
                    print("Headshot!")  -- كل إصابة تعتبر هيدشوت
                    t.isAlive = false
                    table.remove(player.bullets, i)
                    break
                end
            end
        end

        if b.x < 0 or b.x > love.graphics.getWidth() or b.y < 0 or b.y > love.graphics.getHeight() then
            table.remove(player.bullets, i)
        end
    end
end

function fireBullet(x, y, angle)
    table.insert(player.bullets, {
        x = x,
        y = y,
        angle = angle,
        speed = 500
    })
end

function love.draw()
    for _, t in ipairs(targets) do
        if t.isAlive then
            love.graphics.setColor(1, 0, 0) -- أحمر
            love.graphics.circle("fill", t.x, t.y, t.radius)
        end
    end
end
