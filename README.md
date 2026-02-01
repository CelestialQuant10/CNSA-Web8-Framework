# Example: CNSA 2.0 Compliant Key Exchange (CRYSTALS-Kyber)
# Note: Requires pqclean or similar library; install via pip if available in your environment.

from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import kyber  # Assuming pqcrystals-kyber integration

# Generate key pair (Alice)
alice_pk, alice_sk = kyber.generate_keypair()

# Encapsulate shared secret (Bob)
ciphertext, shared_secret_bob = kyber.encapsulate(alice_pk)

# Decapsulate to retrieve shared secret (Alice)
shared_secret_alice = kyber.decapsulate(ciphertext, alice_sk)

# Verify shared secrets match (for demo)
assert shared_secret_alice == shared_secret_bob

# Derive symmetric key using SHA-384 (CNSA compliant)
digest = hashes.Hash(hashes.SHA384())
digest.update(shared_secret_alice)
symmetric_key = digest.finalize()[:32]  # Truncate to 256 bits for AES-256

print("Symmetric Key (hex):", symmetric_key.hex())
