#include <SFML/Graphics.hpp>
#include <cstdlib>
#include <ctime>

// Define constants for the game
const int WINDOW_WIDTH = 800;
const int WINDOW_HEIGHT = 600;
const int PLAYER_SIZE = 50;
const int ENEMY_SIZE = 50;
const int SPEED = 5;

int main() {
    // Create the main game window
    sf::RenderWindow window(sf::VideoMode(WINDOW_WIDTH, WINDOW_HEIGHT), "2D Simple Game");

    // Initialize random seed
    srand(static_cast<unsigned int>(time(0)));

    // Create the player (a red square)
    sf::RectangleShape player(sf::Vector2f(PLAYER_SIZE, PLAYER_SIZE));
    player.setFillColor(sf::Color::Red);
    player.setPosition(WINDOW_WIDTH / 2, WINDOW_HEIGHT / 2);  // Start at the center of the window

    // Create the enemy (a blue square)
    sf::RectangleShape enemy(sf::Vector2f(ENEMY_SIZE, ENEMY_SIZE));
    enemy.setFillColor(sf::Color::Blue);
    enemy.setPosition(rand() % (WINDOW_WIDTH - ENEMY_SIZE), rand() % (WINDOW_HEIGHT - ENEMY_SIZE));

    // Set the frame rate limit
    window.setFramerateLimit(60);

    // Main game loop
    while (window.isOpen()) {
        sf::Event event;
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed) {
                window.close();
            }
        }

        // Move the player with arrow keys
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Up)) {
            player.move(0, -SPEED);
        }
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Down)) {
            player.move(0, SPEED);
        }
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Left)) {
            player.move(-SPEED, 0);
        }
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Right)) {
            player.move(SPEED, 0);
        }

        // Move the enemy randomly
        enemy.move(rand() % 3 - 1, rand() % 3 - 1);  // Random movement (left, right, up, or down)

        // Ensure the player stays within the window
        if (player.getPosition().x < 0) player.setPosition(0, player.getPosition().y);
        if (player.getPosition().x > WINDOW_WIDTH - PLAYER_SIZE) player.setPosition(WINDOW_WIDTH - PLAYER_SIZE, player.getPosition().y);
        if (player.getPosition().y < 0) player.setPosition(player.getPosition().x, 0);
        if (player.getPosition().y > WINDOW_HEIGHT - PLAYER_SIZE) player.setPosition(player.getPosition().x, WINDOW_HEIGHT - PLAYER_SIZE);

        // Check for collision between player and enemy
        if (player.getGlobalBounds().intersects(enemy.getGlobalBounds())) {
            // Reset enemy position and print a message
            enemy.setPosition(rand() % (WINDOW_WIDTH - ENEMY_SIZE), rand() % (WINDOW_HEIGHT - ENEMY_SIZE));
            std::cout << "Collision Detected! Enemy Repositioned." << std::endl;
        }

        // Clear the screen and draw everything
        window.clear(sf::Color::White);
        window.draw(player);
        window.draw(enemy);

        // Display everything we just drew
        window.display();
    }

    return 0;
}
