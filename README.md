#include <linux/module.h>
#include <linux/platform_device.h>
#include <linux/gpio/consumer.h>
#include <linux/of.h>

static struct gpio_desc *led_gpio;

static int my_led_probe(struct platform_device *pdev) {
    led_gpio = gpiod_get(&pdev->dev, "led", GPIOD_OUT_LOW);
    if (IS_ERR(led_gpio))
        return PTR_ERR(led_gpio);

    printk(KERN_INFO "Coursera Assignment 4: LED GPIO found andmed\n");
    gpiod_set_value(led_gpio, 1);
    msleep(500);
    gpiod_set_value(led_gpio, 0);
    return 0;
}

static const struct of_device_id my_led_of_match[] = {
    { .compatible = "coursera,led-gpio" },
    { }
};
MODULE_DEVICE_TABLE(of, my_led_of_match);

static struct platform_driver my_led_driver = {
    .probe = my_led_probe,
    .driver = {
        .name = "coursera-led",
        .of_match_table = my_led_of_match,
    },
};

module_platform_driver(my_led_driver);

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Azim Darxonov");
MODULE_DESCRIPTION("Coursera Assignment 4 Part 1 – Device Tree GPIO LED Driver");
