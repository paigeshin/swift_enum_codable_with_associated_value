# swift_enum_codable_with_associated_value

```swift
enum MessageType: Codable, Equatable, Hashable {
        case purpose(Purpose)
        case affirmationMessageGroup(AffirmationMessageGroup)
        
        private enum CodingKeys: CodingKey {
            case type
            case value
        }
        
        // Enum with raw values will have an automatically generated initializer
        // init(rawValue:), so we can use it to determine the case.
        enum `Type`: String, Codable {
            case purpose
            case affirmationMessageGroup
        }

        init(from decoder: Decoder) throws {
            let container = try decoder.container(keyedBy: CodingKeys.self)
            let type = try container.decode(`Type`.self, forKey: .type)
            
            switch type {
            case .purpose:
                let decodedValue = try container.decode(String.self, forKey: .value)
                self = .purpose(Purpose(rawValue: decodedValue)!)
            case .affirmationMessageGroup:
                let affirmationsValue = try container.decode(AffirmationMessageGroup.self, forKey: .value)
                self = .affirmationMessageGroup(affirmationsValue)
            }
        }
        
        func encode(to encoder: Encoder) throws {
            var container = encoder.container(keyedBy: CodingKeys.self)
            switch self {
            case .purpose(let purposesValue):
                try container.encode(`Type`.purpose, forKey: .type)
                try container.encode(purposesValue, forKey: .value)
            case .affirmationMessageGroup(let affirmationsValue):
                try container.encode(`Type`.affirmationMessageGroup, forKey: .type)
                try container.encode(affirmationsValue, forKey: .value)
            }
        }
        
    }
    
    struct AffirmationMessageGroup: Codable, Equatable, Hashable {
        var id: String
        var title: String
        var messages: [String]
    }
```
