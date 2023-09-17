# swift_enum_codable_with_associated_value

```swift
    enum MessageType: Codable, Equatable, Hashable {
        case purposes([Purpose])
        case affirmations([String])
        
        // Enum with raw values will have an automatically generated initializer
        // init(rawValue:), so we can use it to determine the case.
        enum Raw: String, Codable {
            case purposes
            case affirmations
        }
        
        private enum CodingKeys: CodingKey {
            case type, value
        }
        
        init(from decoder: Decoder) throws {
            let container = try decoder.container(keyedBy: CodingKeys.self)
            let type = try container.decode(Raw.self, forKey: .type)
            
            switch type {
            case .purposes:
                let purposesDecodedValue = try container.decode([String].self, forKey: .value)
                let purposesValue = purposesDecodedValue.compactMap { Purpose(rawValue: $0) }
                self = .purposes(purposesValue)
            case .affirmations:
                let affirmationsValue = try container.decode([String].self, forKey: .value)
                self = .affirmations(affirmationsValue)
            }
        }
        
        func encode(to encoder: Encoder) throws {
            var container = encoder.container(keyedBy: CodingKeys.self)
            
            switch self {
            case .purposes(let purposesValue):
                try container.encode(Raw.purposes, forKey: .type)
                try container.encode(purposesValue, forKey: .value)
            case .affirmations(let affirmationsValue):
                try container.encode(Raw.affirmations, forKey: .type)
                try container.encode(affirmationsValue, forKey: .value)
            }
        }
        
    }
```
