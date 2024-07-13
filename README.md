# robot-collisions
class Solution {
    static class Robot {
        int position;
        int health;
        char direction;
        int index;

        Robot(int position, int health, char direction, int index) {
            this.position = position;
            this.health = health;
            this.direction = direction;
            this.index = index;
        }
    }
    public List<Integer> survivedRobotsHealths(int[] positions, int[] healths, String directions) {
       int n = positions.length;
        List<Robot> robots = new ArrayList<>();
        
        // Create robots and add to the list
        for (int i = 0; i < n; i++) {
            robots.add(new Robot(positions[i], healths[i], directions.charAt(i), i));
        }

        // Sort robots by their positions
        robots.sort(Comparator.comparingInt(r -> r.position));
        
        Deque<Robot> stack = new ArrayDeque<>();
        
        for (Robot robot : robots) {
            if (robot.direction == 'R') {
                stack.push(robot);
            } else {
                while (!stack.isEmpty() && stack.peek().direction == 'R' && robot.health > 0) {
                    Robot rightRobot = stack.pop();
                    if (rightRobot.health > robot.health) {
                        rightRobot.health--;
                        robot.health = 0;
                        stack.push(rightRobot);
                    } else if (rightRobot.health < robot.health) {
                        robot.health--;
                    } else {
                        rightRobot.health = 0;
                        robot.health = 0;
                    }
                }
                if (robot.health > 0) {
                    stack.push(robot);
                }
            }
        }

        // Collect remaining health in original order
        int[] remainingHealths = new int[n];
        Arrays.fill(remainingHealths, -1);
        for (Robot robot : stack) {
            remainingHealths[robot.index] = robot.health;
        }

        // Collect result in the original order
        List<Integer> result = new ArrayList<>();
        for (int health : remainingHealths) {
            if (health != -1) {
                result.add(health);
            }
        }
        
        return result; 
    }
}
