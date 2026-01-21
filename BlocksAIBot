#import <UIKit/UIKit.h>
#import <Foundation/Foundation.h>

@interface BlocksAIBotUI : UIView
@property (nonatomic, strong) UITextField *queryInput;
@property (nonatomic, strong) UITextView *responseDisplay;
@property (nonatomic, strong) UIButton *askButton;
@end

@implementation BlocksAIBotUI

- (instancetype)initWithFrame:(CGRect)frame {
    self = [super initWithFrame:frame];
    if (self) {
        // Aesthetic: Dark semi-transparent background for the bot interface
        self.backgroundColor = [UIColor colorWithRed:0.1 green:0.1 blue:0.1 alpha:0.9];
        self.layer.cornerRadius = 12;
        self.clipsToBounds = YES;

        // Input Field
        self.queryInput = [[UITextField alloc] initWithFrame:CGRectMake(15, 45, 180, 35)];
        self.queryInput.placeholder = @"Ask BlocksAI...";
        self.queryInput.backgroundColor = [UIColor whiteColor];
        self.queryInput.borderStyle = UITextBorderStyleRoundedRect;
        [self addSubview:self.queryInput];
        
        // Ask Button
        self.askButton = [UIButton buttonWithType:UIButtonTypeSystem];
        self.askButton.frame = CGRectMake(205, 45, 80, 35);
        [self.askButton setTitle:@"Execute" forState:UIControlStateNormal];
        [self.askButton setBackgroundColor:[UIColor systemBlueColor]];
        [self.askButton setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];
        self.askButton.layer.cornerRadius = 5;
        [self.askButton addTarget:self action:@selector(fetchBlocksAIResponse) forControlEvents:UIControlEventTouchUpInside];
        [self addSubview:self.askButton];
        
        // Response Area
        self.responseDisplay = [[UITextView alloc] initWithFrame:CGRectMake(15, 95, 270, 190)];
        self.responseDisplay.editable = NO;
        self.responseDisplay.text = @"BlocksAIBot Ready.";
        self.responseDisplay.backgroundColor = [UIColor colorWithWhite:0.2 alpha:1.0];
        self.responseDisplay.textColor = [UIColor greenColor]; // "Hacker/Bot" aesthetic
        self.responseDisplay.font = [UIFont fontWithName:@"Courier" size:12];
        self.responseDisplay.layer.cornerRadius = 8;
        [self addSubview:self.responseDisplay];
    }
    return self;
}

- (void)fetchBlocksAIResponse {
    NSString *key = @"AIzaSyBAR3fAzw6FB5LASUTzssfwJuBSd0NOuOo";
    NSString *userText = self.queryInput.text;
    
    if ([userText length] == 0) return;

    self.responseDisplay.text = @"BlocksAIBot is thinking...";
    
    NSURL *url = [NSURL URLWithString:[NSString stringWithFormat:@"https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=%@", key]];
    
    NSMutableURLRequest *req = [NSMutableURLRequest requestWithURL:url];
    [req setHTTPMethod:@"POST"];
    [req setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
    
    NSDictionary *payload = @{
        @"contents": @[@{
            @"parts": @[@{@"text": userText}]
        }]
    };
    
    NSData *postData = [NSJSONSerialization dataWithJSONObject:payload options:0 error:nil];
    [req setHTTPBody:postData];
    
    [[[NSURLSession sharedSession] dataTaskWithRequest:req completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
        if (data) {
            NSDictionary *json = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];
            @try {
                NSString *aiText = json[@"candidates"][0][@"content"][@"parts"][0][@"text"];
                dispatch_async(dispatch_get_main_queue(), ^{
                    self.responseDisplay.text = aiText;
                });
            } @catch (NSException *e) {
                dispatch_async(dispatch_get_main_queue(), ^{
                    self.responseDisplay.text = @"Error: BlocksAI failed to parse response.";
                });
            }
        }
    }] resume];
}

@end
