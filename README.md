# is.moany
 users (
  id UUID PRIMARY KEY,
  email TEXT UNIQUE,
  google_id TEXT UNIQUE,
  display_name TEXT,
  phone TEXT,
  created_at TIMESTAMP,
  is_verified BOOL,
  kyc_status ENUM('none','pending','approved','rejected'),
  balance NUMERIC(18,8)
)

events (
  id UUID,
  sport TEXT,
  league TEXT,
  home_team TEXT,
  away_team TEXT,
  start_time TIMESTAMP,
  status ENUM('upcoming','live','finished','cancelled')
)

markets (
  id UUID,
  event_id UUID REFERENCES events(id),
  name TEXT,          -- e.g., "Match Winner", "Total Goals Over/Under"
  is_live BOOL,
  created_at TIMESTAMP
)

outcomes (
  id UUID,
  market_id UUID REFERENCES markets(id),
  name TEXT,         -- e.g., "Home", "Draw", "Away", "Over 2.5"
  odds NUMERIC(10,4) -- decimal odds or fractional representation
)

bets (
  id UUID,
  user_id UUID REFERENCES users(id),
  market_id UUID,
  outcome_id UUID,
  stake NUMERIC(18,8),
  odds_at_placement NUMERIC(10,4),
  status ENUM('pending','won','lost','void','cancelled'),
  placed_at TIMESTAMP,
  settled_at TIMESTAMP
)

transactions (
  id UUID,
  user_id UUID REFERENCES users(id),
  type ENUM('deposit','withdraw','bet_stake','bet_return','fee'),
  amount NUMERIC(18,8),
  method TEXT,  -- e.g., 'stripe','crypto','cash_01152787592'
  reference TEXT,
  created_at TIMESTAMP
)
